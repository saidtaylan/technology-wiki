# Unit Test (Jest)

## Unit Test

> Kod içindeki parçaların, diğer parçalardan bağımsız(no dependency) test edilmesidir. Herhangi bir IO işlemi kullanılmaz. Örneğin o parça icinde veritabanına erişim, request okuyup response gönderme gibi işlemler olamz. Sadece mantık test edilir.
> 

### Faydaları

> - Güven veriyor. Sezgisel olarak değil algoritmik olarak kesinlikle doğru olduğundan emin oluyorsunuz.
- Kodunuzu okuyanların, kodlara hangi açılardan yaklaşdığınızı, beklentilerinizin ne olduğunu gösterir.
- 3. parti bir kütüphaneyi yeni versiyonuna upgrade ettiğimizde kodumuzun bozulup bozulmadığını da söyler. Eğer yeni versiyona geçtikten sonra test fail olursa yeni versiyon ile kodumuzda uyuşmayan bir şeyler olduğunu anlarız.
- Solid prensipleri, dependency injection gibi önemli kavramlara daha fazla uymaya başlarız ve böylece kod kalitemiz anlar. Bakış açımızı geliştirir.
- Kodda bir değişiklik yapıldığında, test edilecek koddan beklenen değer ile gelen değer değişeceği için kodda bir değişiklik yapıldığı anlaşılır
- Bir IO işlemi olmayacağı için kodun doğruluğu çok hızlı test edilip feedback alınır.
> 

<aside>
💡 Elbette test edilen kodların başka fonksiyonlara bağımlı olması muhtemel.
Bunun için o bağımlılıkların fake’leri ile yer değiştirilir.
Aksi takdirde o bağımlılıklar da test edilir ve bu uzar gider. Amacından sapar.

Fake için kullanılan iki yapı vardır: Stub and Mock.
Örneğin br futbol sahasının sulanıp sulanmadığını anlamak için iki şey yaparsınız. Ya gidip çimlere, ıslak mı ? diye bakarsınız. Yahut sulama cihazlarına bir sensör takıp çalışıpı çalışmadığını kontrol edersiniz.

Stub, çimlerin ıslaklığını elle kontrol etmektir. Örneğin bir DB’ye veri yazılıp yazılmadığını kontrol etmek için gidip DB’ye bakar. Ancak bu unit test’in mantığına terstir çünkü burada bir IO işlemi var. Çağrılıp çağrılmadığı önemsizdir. Gidip sadece işini yapar.

Mock ise çimlerin ıslaklık kontrolü için sensör takmaktır. Örneğin veritabanına verinin yazılıp yazılmadığını anlamak için, veritabanına veri yazan fonksiyonun çalışıp çalışmadığına bakar. Ve ayrıca o fake veri hakkında da bize bilgier sağlarken stub sağlamaz.

</aside>

# JEST FRAMEWORK

> Örneğin bir toplama fonksiyonumuz olsun ve bu fonksiyonun doğru çalışıp çalışmadığını test edelim. Bunun için jest kütüphanesi import edilip fonksiyonlar çalışttırılmaz. Jest kütüphanesindeki fonksiyonlar kullanılarak test fonksiyonları yazılır. Daha sonra konsoldan **jest** komutu kullanılır. Bu komut, uzantısı **test**.**js**/**test**.**ts** olan dosyaları tarar ve oradaki test fonksiyonlarını çalıştırır.
> 

```jsx
// function.test.js
**const sum = require("./functions");

it("should 1 + 2 = 3", () => {
  const result = sum(1, 2);
  expect(result).toBe(3);
});
/*
It** fonksiyonu test fonksiyonudur. İlk parametre olarak test ismi alır. Genel
kullanım, **should** ile başlamasıdır. Böylece ingilizce mantıklı bir cümle kurulur.
****İstenilen parametrelerle fonksiyon çalıştırılır. Çalışan fonksiyondan dönen
değer(expect) fonksiyondan dönmesi beklen değer(toBe) ise test başarılıdır.
*/
```

```json
// package.json
"scripts": {
    "test": "jest"
  },
```

```jsx
// DECSCRIBE

// Describe, it'leri gruplamaya ve grup test yapmaya olanak verir. It
// aynı şekilde çalışır.

describe("mathematical function tests", () => {
	**it("should 1 + 2 = 3", () => {
	  const result = sum(1, 2);
	  expect(result).toBe(3);
	});**
})
```

```jsx
// ToBe and ToEqual

describe("grouping test", () => {
  it("should 1 + 2 = 3", () => {
    const result = sum(1, 2);
    expect(result).toBe(3);
  });

// Objeler asla aynı olamayacağı için toBe burada çalışmaz.
// Ama toEqual çalışır
  it("should {} = {}", () => {
    const obj = {};
    const array = [];
    expect(array).toEqual([]);
  });
});
```

```jsx
describe("thruthy values", () => {
	**it("should 1 + 2 = 3", () => {
	  const value = ""
	  expect(result).toTruthy()
	});**
})

describe("falsy values", () => {
	**it("should 1 + 2 = 3", () => {
	  const value = "said"
	  expect(result).toFalsy()
	});**
})

describe("undefined values", () => {
	**it("should 1 + 2 = 3", () => {
	  const value = undefined
	  expect(result).toBeUndefined()
	});**
})

describe("null values", () => {
	**it("should 1 + 2 = 3", () => {
	  const value = null
	  expect(result).toBeNull()
	});**
})
```

```jsx
Diziler, stringler, float'lar hatta hatalar üzerinde bile test yapılabilmektedir.
```

```bash
# Eğer sadece bütün dosyaları değil de belli dosyaları test etmek istiyorsak
# test edilecek dosyanın adını, test eklenitisi olmadan yazmalıyız.
npm run test testedilecekDosyaAdı
```

```bash
beforeEach(() => {something()}) ve afterEach(()=> {}) sayesinde  her testin
öcnesine ve sonrasında bir şey çalıştırılabilir
```

```bash
beforeAll(()=> {}) ise testlerin en başında bir kez çalışır ve biter.
Sonra beforeEach varsa başlar
```

```bash
Eğer after ve beforEach'ı tüm testlere değil de sadece belirli bir gruba
uygulamal istersek o zamand describe metodunun içinde çalıştırmak gereklidir.
```

```bash
it.only() diyerek sadece o testin çalışmasını sağlayabiliriz
```

![Untitled](Unit%20Test%20(Jest)%202ef5f0b3ab18466da755a0adf063e809/Untitled.png)

<aside>
💡 Mocks: Bir uzun metodumuz olabilir. Bu uzun metotun neresinde patladığını görebilmek için fonksiyonların her biri test edilir. Böylece hangi fonksiyonun neresinde sorun olduğu belli olur. Örneğin yukarıdaki örnekte forEach fonksiyonu yazılmış. Bu fonksiyon forEach’ın yaptığı görevi yapıyor.
Mock ise, forEach fonksiyouna gönderilen callback fonksiyonunu bir mock fonksiyonu olarak yazıp öyle göndermemizi istiyor. Böylece aslında mock bizim ajanımız olacak ve forEach fonksiyonunda callback çalışırken gidiştaı gözlemleyebileceğiz.

</aside>

<aside>
💡 mock.calls ile her bir fonkisyon çağrısındaki ayrıntıları görebiliriz.

</aside>

![Untitled](Unit%20Test%20(Jest)%202ef5f0b3ab18466da755a0adf063e809/Untitled%201.png)

<aside>
💡 Yukarıda olduğu gibi calls yerine result ile de direkt olarak results’e ulaşabiliriz.

</aside>