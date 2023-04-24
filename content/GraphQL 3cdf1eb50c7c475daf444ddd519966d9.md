# GraphQL

# Nedir ne işe yarar?

Api sorguları atmamızı sağlar. Bir yöntemdir.

# REST’ten ne farkı var?

- Normal sorgular için aman aman bir farkı yok.
- Mesela kullanıcı kendi profil resmini almak istiyor. Bunun için elbette bir endpoint oluşturulabilir. Ancak sistemde sürekli farklı fotolar için farklı endpointler oluşturmak zahmetlidir. Ya da front-end’e, “ben sana bütün fotoları gönderiyom, al sen filtrele, ihtiyacın olanı bul” diyecek. Bunun ise mantıksızlığı anlaşılabiliyor. GraphQL ise /images endpointine istek atıp sadece gerekli resmin ismini ya da id bilgisini verip çekebilir.
- Bir kullanıcımız modülümüz olduğunu varsayalım. Bu modülde kullanıcının farklı farklı bilgilerini çekmek için REST’te farklı endpoint’ler tanımlamak zorundayız. Beğenileri getir, yorumları getir, arkadaşlarını getir, bilgilerini getir...
Ancak bu işlem back-end’ciyi yoracaktır. Front-end’e last_login isimli bir bilgi gerekiyor ve bunu endpointinin yazılması gerekiyor. Ancak back-end’ci o anlık müsait değildir ve bu yüzden iş gecikir. Ancak graphQL ile tek yapılması gereken, kullanıcının istek atacağı endpoint’te last_login bilgisini expose etmek.
- Buraya kadar çok bir fark yok. Bunlark REST’te de kolayca yapılabilir.
- Ancak büyük bir fark var:
- Nested Queries:

Bir kullanıcının arkadaşlarının ilk isimlerini çekmek istiyoruz diyelim. REST’te bunun için ilk olarak kullanıcının arkadaşlarını bulup onları bir dizi şeklinde döndürmek, daha sonra o herbir arkadaş için istek atıp onların isimlerini almak gerekiyor. Bu ise kaynak israfı oluyor. GraphQL ise ne yapıyor

```graphql
me {
	friends {
		name
	}
}
# Yani ne yaptık? Kolay bir şekilde bu bilgileri çekebildik.
```

- Mutations ile update/delete/create yapılabiliyor
- Çekilen verilere aliasing yapılabiliyor. Çekilecek içeriği aynı olan iki alan aynı ise graphQL buna izin vermz. Bunun için alias kullanırız.
- Kullanıcı giriş yapmışsa başka şema giriş yapmamışsa başka şema bile dönülebilir.
- Subscriptions
- Gragment: Bildiğin interface. Birden çok yerde kullanılacak field’ları bir scope altında toplayıp kullanabiliyorsun.
- Query’e argüman geçebilir. query($n: Int=5) {name}
- Directive’ler @ ile başlar. Ve @include mesela; argüman olarak geçirilen değer eğer condition’da sağlanıyorsa içerideki sorguyu yapar.