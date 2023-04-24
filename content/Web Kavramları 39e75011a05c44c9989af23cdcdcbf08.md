# Web Kavramları

## LocalStorage

Yalnızca client-side’da geçerli olan ve server tarafından erişilemeyen, tarayıcıda saklanan bilgilerdir.

Text şeklinde tutulur.

5 MB depolama sınırı vardır.

Yalnıızca o domain(site) tarafından erişilebilir.

Süresiz şekilde depolanırlar.

## SessionStorage

Sunucu tarafından erişilebilen ve yönetilebilen, tarayıcıda saklanan bilgilerdir. 

LocalStorage’dan farkı, sunucu taraflı olması dışında tarayıcıdaki her bir sekme başına bir store oluşturulmasıdır.

Adının hakkını vererek gerçekten bir oturum oluşturur.

Aynı site dahi olsa başka bir sekme açıldığında o sekme için yeni bir oturum oluşturulur.

Session’un bulunduğu sekme yenilense de store sıfırlanmaz.

Sekme kapatıldığında store sıfırlanır.

## Cookie

**Server-side cookie, genelde session olarak bilinir.**

Server tarafından erişilebilen ve client tarafında depolanan bilgilerdir.

Süreli olarak tutulurlar.

Sunucu tarafından oluşturup server tarafından erişilebilir. Veya tam tersi olarak client tarafından oluşturuup server tarafından erişilebilir.

Genelde kullanıcı doğrulama için kullanılır. Örneğin kullanıcı sisteme giriş yaptığında kullanıcının email ve şifresi cookie olarak istemciye gönderilir. İstemciden, her giriş gerektiren işlem yapıldığında header içinde cookies objesi içinde sunucuya gönderilir.

Path’a özel olarak cookie oluşturulabilir.

HttpOnly Cookie, sadece sunucu tarafından yazılabilir ve okunabilir. Böylece XSS ataklarından kaçınılır.

Başka sitelerden bu cookie’lere erişilememesi için sameSite attribute’si strict yapılmalıdır ki CSRF açığı ortaya çıkmasın.

---

[Local Storage vs Cookies](https://stackoverflow.com/questions/3220660/local-storage-vs-cookies)

---

## OAuth 2.0

Uygulamaların, başka uygulamaların kaynaklarına erişmesini sağlayan bir protokoldür.

Örneğin ben uygulamamda Google ile giriş yap’ı kullanmak istiyorsam OAuth kullanmalıyım. Çünkü Google bana ve ben Google’a OAuth aracılığı ile güveneceğim.

Burada bazı terimler var:

Client: Kendi uygulamamda bir kullanıcı olan ben

Authorization server: Google’a yani istenen uygulamaya gidip izin isteyen ve ve eğer izin verirse geriye bir Authorization code dönen server

Resource owner: İzin almak istediğimiz uygulamanın sahibi

Resource server: İzin almak istediğimiz uygulamanın kendisi.

![Untitled](Web%20Kavramlar%C4%B1%2039e75011a05c44c9989af23cdcdcbf08/Untitled.png)

## Subnet

```bash
156.2.5.8/24
```

Yukarıdaki 24 şu demektir:

En yüksek dereceli 24 bit sabittir.

Her bir nokta arası 8 bit olduğundan aslında 3 nokta arası sabittir yani sadece 4. üçlü kısım değişebilir. Bu da 2^8’den 255 olabilir. Yani bu subnette en fazla 255 ip adresi olabilir.

Eğer 24 değil de 25 olsaydı en yüksek dereceli ilk 25 bit sabit olacaktı. Bu yüzden 8*4 =32-24=7 olur. Geriye kalan 7 bit ile max 2^7=127 ip adresi verilebilir.

### Default Gateway

Ulaşılmak istenen ip adresi eğer belirlenen ağ içinde yoksa; paket, default olarak belirlenen bir ip adresine yönlendirilir.