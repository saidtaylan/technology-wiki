# Vue3 Bootcamp

- v-bind
- function binding
- instance creating
- event binding(@click)
- Two way data binding
- Computed and watch
- life cycle hooks
- class binding
- style binding
- conditions
- loop - list rendering
- Array watcher ⇒ diziler direk obje notasyonunda izlenemez. Değişkeni obje olarak açıp içine property olarak 'deep:true' verip 'handler' diye bir metod ekleyip watcher'i handler metodunda kullanıyoruz. Ör:

```jsx
itemList: {
deep:true,
handler(itemList) {console.log("itemList dizisi değişti")}
}
```

- Component nedir? Neden kullanırız?
- Template elementi içinde kök bir element olmak zorunda değildir.
- Global component
- Props
- send event with emit(child to parent)
- prop ile function gonderilebilir
- provide/ inject (provide edilecek Data için bir obje oluşturulur. Bütün datalar bu objenin içinde oluşturulur.

```jsx
data(){
return {
	providedData: {
		username: saidtaylann
}
}
}

provide: {
return :{
	providedData: this.providedData
}
}
```

- Emit dökümantasyonu. Tipi array olan "emits" adında bir property ile, component içinde hangi nelerin emit'lendiğini görebilir. Herhangi bir işlevi yoktur, sadece kolay okunur.
- Slot'lar ⇒ Bir component tagları arasına bir şey yazılırsa bu slot olarak kabul edilir. Yazılan component'in uygun yerinde <slot> elementi varsa bu elementin yerine yazılan html kodu gelir. Eğer componentin içinde <template> elementi açılıp içinde istenen html kodu yazılırsa ve bu template'ye (# işaretiyle )bir name bilgisi(<template #title>) verilirse named slot kullanılmış olur. Kullanmak istediğim componentin dosyasında, html kodunu kullanmak istediğimiz yere <slot name="slot-name"/> yazarsak kodu aktarmış oluruz.

```jsx
<modal>
	<template #title>
		<div> HTML CONTENT HERE </div>
	</template>
</modal>

#modal component
<div>
	<slot name="title"/>
</div>
```

- Dynamic components. <component> ve <keep-alive>. Activated and deactivated hooks
- Axios ve json-server
- vuex, getters, state, mapgetters, mapp getters with spread operator

```jsx
computed: {...mapGetters(['getter1', 'getter2'])}
computed: {...mapGetters({getter1)}
```

- mutations, actions
- Actions, parametre olarak context alır ve bu vuex bilgilerini tutar.

```jsx
actions: {
	setProduct(context, item){
		axios.post(...).then(res=> {context.commit("addProduct")})
	}
	setProduct2({commit}, item){
		axios.post(...).then(res=> {commit("addProduct")})
	}
}
// süslü parantezle yani destructuring parameter olarak da context elemanlarını alabiliriz.
```

- Vuex'de modüller yapılabilir. Users, products, tasks gibi modüller oluşturabiliriz ve bunu createStore içinde modules property'sinde kullanırız.

```jsx
// store.js
import {createStore} from "vuex"
import user from "./modules/user"
const store = createStore({
	state: {},
	getters: {},
	modules: {
		user: user,
		product: product
	}
})

//user.js
export defafult {
	getters: {
		username: '',
		firstName: '',
		lastName: '',
		avatar: ''
	},
	getters: {
		getfullName(state) {
				return state.firstName+" "+state.lastName	
		}
	}
}
export default store
//main.js
import store from store
app.use(store)
```

- vue-router@4

```jsx
//router.js
import {createRouter, createWebHistory()} from "vue-router"

const routes = [
	{
		path: '/',
		component: () => import "@/component/Home"
	},
]

cosnst router = createRouter(
	routes: routes,
	history: createWebHistory()
)
exprot default router
//main.js
import router from "router"
app.use(router)

// app.vue
<template>
	<router-view></router-view>
</template>

```

- router-link

```jsx
<router-link active-class="active" to="/" class="nav-link">Anasayfa</router-link>
```

- dynamic routing and naming and parameter

```jsx
// router.js
{name:'home', path: '/:id', component: Home}
// details.vue
<p> {{$route.params.id}}</p>
this.$router.push({name: "home"})
<router-link to=`/${id}` active-class="active"/>
```

- Hash History: Proje yapılıp sunucuya atıldığında(ör: engineX) sunucu bunun bir SPA olduğunu anlayamıyor. Route'e gidip bakıyor anca öyle bir sayfa bulamıyor. Ancak # işareti gördüğünde bunu bir SPA olduğunu anlıyor. Eğer "hash" modu kullanılmak istenmiyorsa sunucu tarafında bir konfigürasyon kodu yazmak gerekiyor.
- Global olarak axios kullanma

```jsx
//main.js
import axios from axios
app.config.globalProperties.$axios = axios
```

- Vite kurulumu ve kullanımı

```jsx
npm init vite@latest
```

- Composition api giriş ve neden kullanılır?
- Vue option api, bütün dataları reactive olarak tanımlar. Reactive olmasına gerek olmayanlar boşuna reaktif olmuş olur. Ancak composition api ile sadece reactive olmasını istediklerimizi reaktif yaparız.
Birbiriyle ilgili olan metod, data, computed ve watcher'ları bir yerde toplamış oluruz. Debug süresini kısaltmış olur.
- Composition basit kullanımı

```jsx
import {ref, computed} from "vue"

setup(){
	const title = ref("This is a title")
	const changeTitle = () => {
		this.title.value = "The other title"
	}
	const titleLength = computed(() => {
		return this.title.value.length + "adet karakter yazdınız."
	})
	const counter = ref(0)
// watch bir değer döndürmeye gerek olmadığın için return etmiyor
// ve bir değişkene atamıyoruz
	watch(counter, (newValue, oldValue) => {
		console.log(oldValue ,"->", newValue)
	})
// eğer birden fazla izlenmek istenen değer olursa
	watch([counter1, counter2], ([c1NewValue, c2NewValue], [c1OldValue, c2OldValue]) => {
		console.log("c1:", c1oldValue, "->", c1NewValue)
	console.log("c1:", c2oldValue, "->", c2NewValue)
	})
	return {title, changeTitle, titleLength}
}
```

- Deep watching: Eğer bir dizi ya da objenin içindeki bir eleman değiştirilirse watcher bunu göremez. Ancak watcher'a bir property olarak deep: true verirsek dizi ve objedeki değişiklikleri de algılayacaktır

```jsx
// composition api
watch(counter, (newValue, oldValue) => {
		console.log(oldValue ,"->", newValue)
	}, {deep: true})
```

- watchEffect: Herhangi bir ögeyi izlemeden sadece içinde yazılan işlemleri yapar.
onInvalidate ise, izlenen değer sürekli aynı olduğunda watcher'i çalıştırmaz. Sadece 1500ms'dedn sonra değer değişmiş ise watcher'i çalıştırır

```jsx
import {watchEffect} from "vue"
setup(){
	watchEffect((onInvalidate)=> {
		if(typingText.value !== ''){
			isTyping.value = true
		}
		const typing = setTimeout(()=> {
			isTyping.value = false
		}, 1500)
		onInvalidate(()=> {
			clearTimeout(typing)
		})
	})
}
```

- Composables: Setup fonksiyonu içindeki api'yi başka bir dosyada yazıp buraya import etme

```jsx
// header.js
import {ref, computed} from "vue"
export default function() {
	const title = ref("This is a title")
	const changeTitle = () => {
		this.title.value = "The other title"
	}
	const titleLength = computed(() => {
		return this.title.value.length + "adet karakter yazdınız."
	})
	return {
		title,
		titleLength
	}
}
//header.vue
import header from "./composables/header.js"
setup(){
	const {title, titleLength} = header()
	return {
		title, titleLength
	}
}
```

- Setup fonksiyonu iki parametre alır. Props ve context. Props, componente gelen propsları ifade eder.
- Script setup

```jsx
//reactive bir veride direk yapılan watch işlemi çalışmaz. Ancak böyle yaparsın:

watch(()=> JSON.parse(JSON.stringfy(state.user)), (newValue, oldValue)=> {} )
```

- Suspense component

```
Eğer bir kaynaktan çektiğimiz bir verimiz varsa ve veri gelene kadar ekran bir loading
bar göstermek istiyorsak bu tam suspense oluyor.
Asenkron işlem yapacak olan componenti suspense component'ine slot olarak veriyoruz.
Veri gelene kadar , ikinci slot olan #fallback'i çalıştırır. Ve veriler gelene kadar
componenti yüklemez.
```

```jsx
<template>
  <suspense>
    <template #default>
      <todo-list />
    </template>
    <template #fallback>
      <div>
        Loading...
      </div>
    </template>
  </suspense>
</template>

<script>
export default {
  components: {
    TodoList: defineAsyncComponent(() => import('./TodoList.vue'))
  }
}
</script>
```

- Teleport component

```
Bir içeriği, direkt olarak belli bir HTML alanında gösterme. Slot olarka falan geçirmen
```

```jsx
// index.html
<div id="modal-area"/>

// myModal.vue
<teleport to="modal-area">
	my sweety modal
</teleport>
```

- Namespaced components

```
Bir componentte çok fazla import olması estetiği bozar ve karmaşıklık oluşturur.
Bunun için componentteki importları ayrı bir js dosyasında tutuup componentte
o dosyayı importluyoruz
```

```jsx
// componentImports.js

export {default as Input} from "Input.vue"
export {default as Form} from "Form.vue"
export {default as Select} from "Select.vue"

//app.vue
<script setup>
import * as Form from "./componentImports.js"
</script>

<template>
<Form.Input/>
<Form.Form/>
<Form.Select/>
<template>
```