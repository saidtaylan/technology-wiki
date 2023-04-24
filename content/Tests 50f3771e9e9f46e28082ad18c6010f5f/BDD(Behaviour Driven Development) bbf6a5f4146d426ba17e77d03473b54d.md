# BDD(Behaviour Driven Development)

<aside>
💡 DDD, bir kodun nasıl çalıştığı ile değil ne yaptığı ile ilgilenir.
DDD, problemin çözümünü değil problemi anlatmakla ilgilenir.

</aside>

TDD’nin problemlerinden ortaya çıkmıştır. Aslında bu, TDD’nin suçu değildir. Bu, TDD’nin doğasında vardır. 

<aside>
💡 TDD tamamen koda bağımlıdır. Bir test yazar ve ardından onun kodunu yazarsınız ya da tam tersi.

Eğer kodda herhangi en küçük bir değişiklik yapacak olsanız dahi testiniz başarısız olacaktır.

</aside>

<aside>
💡 Örneğin VueJS 2’de bir test yazdınız ve bunun üzerinden yıllar geçti. Vue 3’te birçok şey değişti ve belki de composition api’ye geçtiniz. Artık o testiniz tamamen geçersiz bir test. Onu yeniden düzenlemelsiniz.

</aside>

<aside>
💡 Daha farklı bir senaryo: Bir Unit Test değil de GUI test yaptığınızı düşünelim. Selenium ile Amazon web sitesine istek attınız. Xpath kullanarak, class ve id isimleri ile gerekli elementlerden verileri toplayıp testinizi yazdınız çok güzel. Ancak Amazon bir class name ya da id name değiştirirse test kodu tamamen çalışmaz olacak. Ve bu test sadece mobil için çalışacak. Hatta tarayıcıya bile bağımlı olacak. Ve hatta bir mobil uygulama için farklı, web app için farklı tetsler yazmak gerekecek.

</aside>

<aside>
💡 Yaptığınızı yani testinizi okumak isteyen birisi kullandığınız teknolojiyi, teknolojinin inceliklerini bilmek zorunda. Örneğin selenium örneğinde XPath’i dahi bilmek zorunda

</aside>

<aside>
💡 - DDD, amaca odaklanır. Kodlarla ilgilenmez
- Herkes tarafından rahatça okunabilir. TDD gibi teknoloji ve terminolojiyi bilmeyi gerektirmez.
- Test detaylarını gizler
- Kodda değişiklikler olsa da değişikliklerden etkilenmez. Örneğin GUI testinde Amazon bir class ismini değiştirse de bir unit test bundan etkilense de DDD bundan etkilenmez çünkü hala problem aynıdır.
- Bakımı oldukça kolaydır
- Platform bağımlı değildir yani mobil için de web için de çalışır
- Kod, dökümante edilmiş olur

</aside>