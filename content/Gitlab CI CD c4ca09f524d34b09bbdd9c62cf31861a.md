# Gitlab CI/CD

<aside>
💡 CI/CD(Continuous Interation), DevOps sürecini otomatize etmeye yarayan bir araçtır.
Örenğin bir repoya bir commi yapıldığında commitlenen kodu, config dosyasında belirtilen şartlara göre gerekirse build, test ve deploy eder.

</aside>

<aside>
💡 Gitlab CI/CD bu işi .gitlab-ci.yml dosyası içinde yapar.

</aside>

## Pipeline

- CI sürecinin tamamlanması için adı üzerinde bir boru hattıdır. Sunucu ile gitlab arasındaki bütün süreçlerin aktığı hattır.
- Bir pipeline iki şeyden oluşur:
    - Stage: Sırasıyla çalışan aşamalardır (build, test, deploy gibi)
    - Job: Her bir aşamada yapılacak işlerdir. Ve her bir stage’deki job’lar concurrently çalışır.
- Pipeline genelde otomatik olarak çalışır. Ancak istendiğinde manuel olarak da çalışabilir.
- Eğer pipeline sürecindeki bir stage’deki işlerin herhangi birinde bir hata oluşursa diğer stage’ye geçimez ve pipeline sonlandırılır.

## Jobs

- Pipeline’ler Jobs ile başlar.
- Job’ın sayısı önemli değildir.
- Hangi şartları altında çalışacağını beelirten bazı kısıtlamalar tanımlanır.
- Her jobs en azından “script” cümlesini içermelidir.
- Joblar birbirinden izole şekilde farklı container’lerde çalıştığı için birbirlerinden izole çalışırlar.

```yaml
job1:
  script: "execute-script-for-job1"

job2:
  script: "execute-script-for-job2"
```

- Job ismi olarak şunlar kullanılamaz:
    - `image`
    - `services`
    - `stages`
    - `types`
    - `before_script`
    - `after_script`
    - `variables`
    - `cache`
    - `include`
    - `true`
    - `false`
    - `nil`
- Job statusları şunlardır:
    - failed
    - warning
    - pending
    - running
    - manual
    - scheduled
    - canceled
    - success
    - skipped
    - created
- Joblar stage’ler ile gruplanabilir.

```yaml
build ruby 1/3:
  stage: build
  script:
    - echo "ruby1"

build ruby 2/3:
  stage: build
  script:
    - echo "ruby2"

build ruby 3/3:
  stage: build
  script:
    - echo "ruby3"
```

- Job isminin başına “.” koyulduğunda bu job, pipeline çalışırken görmezden gelini

```yaml
.hidden_job:
  script:
    - run test
```

- Default olarak tanımlanan değerler, job’lar tarafından inherit edilebilir.

```yaml
default:
  image: 'ruby:2.4'
  before_script:
    - echo Hello World

variables:
  DOMAIN: example.com
  WEBHOOK_URL: https://my-webhook.example.com

rubocop:
  inherit:
    default: false
    variables: false
  script: bundle exec rubocop

rspec:
  inherit:
    default: [image]
    variables: [WEBHOOK_URL]
  script: bundle exec rspec

capybara:
  inherit:
    variables: false
  script: bundle exec capybara

karma:
  inherit:
    default: true
    variables: [DOMAIN]
  script: karma
```

## Variables

- YML dosyası içinde tekrar kullanılabilen değişkenler tanımlanabilir.

```yaml
variables:
  SA_PASSWORD: $SA_PASSWORD
```

- Gitlab tarafından tanımlı olarak gelen bazı değişkenler de vardır ve bunlar dosyada belirtilmeksizin kullanılabilir.
- Job içinde local variables de tanımlanabilir.

```yaml
variables:
  TEST_VAR: "All jobs can use this variable's value"

job1:
  variables:
    TEST_VAR_JOB: "Only job1 can use this variable's value"
  script:
    - echo "$TEST_VAR" and "$TEST_VAR_JOB"
```

- Global olarak tanımlanmış değişkenler, bir Job içinde kullanılsın istemiyorsak:

```yaml
job1:
  variables: {}
  script:
    - echo This job does not need any variables
```

- Değişken içinde değişken kullanılabilir.

```yaml
job:
  variables:
    FLAGS: '-al'
    LS_CMD: 'ls "$FLAGS"'
  script:
    - 'eval "$LS_CMD"'  # Executes 'ls -al'
```

- Değişkenlerin içinde, $ işaretinin yorumlanmasını yani değişken işareti olarak kullanılmasını istemiyorsak iki tane yan yana $ işareit kullanabiliriz.

```yaml
job:
  variables:
    FLAGS: '-al'
    LS_CMD: 'ls "$FLAGS" $$TMP_DIR'
  script:
    - 'eval "$LS_CMD"'  # Executes 'ls -al $TMP_DIR'
```

- Projeye değişken eklemek için **Settings > CI/CD** altındaki **Variables** sekmesinden değişkenler oluşturulabilir. Yukarıdaki örneklerdeki değişkenler, CI dosyasını okuyan herhangi birisi tarafından görülebilir. Ancak projeye eklenen değişkenler repo yetkilisi haricinde görülemez.
Prode değişkeni ekleme sekmesinde bazı kavramlar vardır:
    
    ```yaml
    Protect variable: Bu değişken sadece protected branch'lerde kullanılabilir.
    Mask variable: Bu değşkenin kullanım esnasında, log'alrda ya da herhangi bir
    							 kullanım yerinde değişkenin değeri maskelenecek yani görünmeyecek.
    Type: Her ne kadar değişken içeriği saf text olarak girilse de tip olarak file
    			seçilirse Gitlab; içeriği, yazılan text olan geçici bir dosya oluşturur.
    			Bu, private key kullanımında iyi bir seçenektir.
    ```
    
- Proje değişkenleri, normal değişkenler gibi dosya içinde kullanılabilir.

```yaml
test_variable:
  stage: test
  script:
    - echo "$CI_JOB_STAGE"  # calls a predefined variable
    - echo "$TEST"          # calls a custom variable of type `env_var`
    - echo "$GREETING"      # calls a custom variable of type `file` that contains the path to the temp file
    - cat "$GREETING"       # the temp file itself contains the variable value
```

## Cache

- İnterrnetten inidirilen dosyaları cache’ye alır.
- Cache runner’da depolanır ve S3’e upload edilir.
- Bir job’da cache edilen dosya CI dosyasının sonraki joblarında kullanılabilir.
- Farklı projeler aynı cache’yi paylaşamaz.
- Job içinde cache keyword’ü ile tanımlanır.

```yaml
test-job:
  stage: build
  cache:
    - key:
        files:
          - Gemfile.lock
      paths:
        - vendor/ruby
    - key:
        files:
          - yarn.lock
      paths:
        - .yarn-cache/
  script:
    - bundle config set --local path 'vendor/ruby'
    - bundle install
    - yarn install --cache-folder .yarn-cache
    - echo Run tests...
```

- Cache’yi devre dışı bırakmak

```yaml
job:
  cache: []
```

```yaml
test-job:
  stage: build
  cache:
    - key:
        files:
          - Gemfile.lock
      paths:
        - vendor/ruby
    - key:
        files:
          - yarn.lock
      paths:
        - .yarn-cache/
  script:
    - bundle config set --local path 'vendor/ruby'
    - bundle install
    - yarn install --cache-folder .yarn-cache
    - echo Run tests...
```

- Go bağımlılıkları cache edilebilir. Böylece sürekli olarak indirmeye gerek kalmaz.

```yaml
.go-cache:
  variables:
    GOPATH: $CI_PROJECT_DIR/.go
  before_script:
    - mkdir -p .go
  cache:
    paths:
      - .go/pkg/mod/

test:
  image: golang:1.13
  extends: .go-cache
  script:
    - go test ./... -v -short
```

## Artifact

- Sadece o pipeline’de kullanılabilece bir depolama alanıdır.
- Job başına artifact tanımlanır.
- Sonraki job’lar bu artifacti kullanabilir.
- Farklı projeler artifacti kulanamaz.
- Default olarak 30 gün depolanırlar. Ancak bu süre(expire_in) değiştirilebilir.

```yaml
pdf:
  script: xelatex mycv.tex
  artifacts:
    paths:
      - mycv.pdf
    expire_in: 1 week
```

- Bütün path’ler, proje repository’sine relative’dir.
- Artifact’i devre dışı bırakmak için dependencies boş bırakılır.

```yaml
job:
  stage: build
  script: make build
  dependencies: []
```

- Wildcard kullanılabilir.

```yaml
job:
  artifacts:
    paths:
      - path/*xyz/*
```

## Gitlab CI File

The scripts are grouped into **jobs**, and jobs run as part of a larger **pipeline**. You can group multiple independent jobs into **stages** that run in a defined order. The CI/CD configuration needs at least one job that is not hidden.

You should organize your jobs in a sequence that suits your application and is in accordance with the tests you wish to perform. To visualize the process, imagine the scripts you add to jobs are the same as CLI commands you run on your computer.

## Docker

- Her job bir Docker image’sidir. Default olarak bu image Ruby’dir.
- Ancak örneği bir Python uygulaması yapıyorsak bu image’yi Python olarak belirtebiliriz ya da kendi oluşturduğumuz bir image’yi de verebiliriz.

### Docker in Docker

- Her job’in bir docker olduğunu söylemiştik. Default olarak da Ruby geliyordu.

<aside>
💡 Peki biz image içinde docker komutu kullanmak istersek ne olacak?
- Bunun için Docker-in-Docker diye bir terim var: Docker içinde Docker Kullanmak
Her job zaten bir docker iken bu docker içindeki image’yi de Docker’in official image’si olarak belirliyoruz. Böylece artık job içinde docker komutu kullanabiliyoruz.

</aside>

```yaml
build_image:
	image: docker:20.10
	before_script:
		- docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
	script:
		- docker build -t $IMAGE_NAME:$IMAGE_TAG .
		- docker push $IMAGE_NAME:$IMAGE_TAG
```

### Services

Eğer bir job içinde birden fazla image gerekiyorsa ana image ile diğer container’lerin iletişebilmesi için servisler oluşturulur. Aynı docker compose’deki servisler gibi. Oluşturulan servisler birbiriyle direkt konuşabilir. Ama bundan emin olmak için bir variable oluşturulur. Bütün container’ler aynı sertifikaya erişim sağlar ve konuşur. 

```yaml
build_image:
	image: docker:20.10.6
	services: docker:20.10.6-dind # daemon tag
	variables:
    DOCKER_TLS_CERTDIR: "/certs"
	before_script:
		- docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
	script:
		- docker build -t $IMAGE_NAME:$IMAGE_TAG .
		- docker push $IMAGE_NAME:$IMAGE_TAG
```

## Services

- İlgili job’ları gruplar.
- Eğer joblara bir stage verilmezse bütün joblar aynı anda çalışacaktır. Stage’ler ise sırayla çalışır. Bu yüzden sırayla çalışmasını istediğimiz job’ları sıralı stage’lere atarız.
- Aynı stage’deki job’lar paralel olarak çalışır.

```yaml
stages:
  - test
  - build
  - deploy

run_tests:
  stage: test
  image: python:3.9-slim-buster
  before_script:
    - apt-get update && apt-get install make
  script:
    - make test

build_image:
  stage: build
  image: docker:20.10.16
  services:
    - docker:20.10.16-dind
  variables:
    DOCKER_TLS_CERTDIR: "/certs"
  before_script:
    - docker login -u $REGISTRY_USER -p $REGISTRY_PASS
  script:
    - docker build -t $IMAGE_NAME:$IMAGE_TAG .
    - docker push $IMAGE_NAME:$IMAGE_TAG
```

## Remote Server Stuffs by SSH

- Eğer digitalocean gibi bir remote server’da bir VPS varsa uzak server’a SSH ile bağlantı yapılabilir. İlk kez VPS’e bağlanılıyorsa bağlantı komutunda private key de belirtilmelidir.

```bash
ssh -i .ssh/private_key root@ip_address
```

- Yeni kurulan bir sunucuya ilk olarak Docker kurmak gereklidir.
- CI sürecinde manuel olarak değil de otomatik olarak bağlantı kuruacağı için(by Gitlab runner) gitlab’e SSH keyi eklemeliyiz. Bunun için Ayarlardan(CI/CD → variables) SSH private key’inin içeriğini eklemeliyiz ki herkes tarafından görünür olmasın. Ayrıca tipi de File olmalıdır çünkü SSH bash’te böyle çalışır.

```bash
deploy:
  stage: deploy
  before_script:
    - chmod 400 $SSH_KEY
  script:
    - ssh -o StrictHostKeyChecking=no -i $SSH_KEY root@161.35.223.117 "
        docker login -u $REGISTRY_USER -p $REGISTRY_PASS &&
        docker ps -aq | xargs docker stop | xargs docker rm &&
        docker run -d -p 5000:5000 $IMAGE_NAME:$IMAGE_TAG"
```

> StrictHostKeyChecking: SSH key doğrulama ekranının gelmesini engeller. Süreci bir insan değil de gitlab sürdürdüğü için gitlab, Yes/no/fingerprint seçeneklerine basamaz. `npm init` yaparken -y flag'ı eklemek gibi düşünülebilir.
> 

> Gitlab, geçici olarak oluşturduğu File değişkenlerinin yetkilerini, “herkes okuyabilir ve yazabilir” olarak ayarlar. Ancak bu, bizim için doğru bir durum değildir. Bunun için before_script ile dosyanın yetksini 400’e(sadece sahibi okuyabilir) düşürüyoruz.
> 

# An Example CI File

[.gitlab-ci.yml · main · Nana Janashia / gitlab-cicd-crash-course · GitLab](https://gitlab.com/nanuchi/gitlab-cicd-crash-course/-/blob/main/.gitlab-ci.yml)