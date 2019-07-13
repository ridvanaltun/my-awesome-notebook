# Vue.js

Vue kendini `progressive` ve `reactive` bir `framework` olarak tanıtıyor. Peki bunun anlamı ne?

# Table of Contents

<!-- toc -->

- [Vue CLI and Vue GUI](#vue-cli-and-vue-gui)
- [File Structure](#file-structure)
- [Basic Logic](#basic-logic)
  * [Expressions](#expressions)
- [Testing](#testing)
- [Features](#features)
  * [Single Page Component](#single-page-component)
  * [Two Way Data Bindings](#two-way-data-bindings)
  * [Attribute Binding](#attribute-binding)
  * [Conditional Rendering](#conditional-rendering)
  * [List Rendering](#list-rendering)
  * [Event Handling](#event-handling)
  * [Class & Style Binding](#class--style-binding)
  * [Computed Properties](#computed-properties)
  * [Components](#components)
- [data() vs data:](#data-vs-data)
- [Libraries](#libraries)
  * [Fetch API](#fetch-api)
  * [Axios](#axios)
  * [Vue Router](#vue-router)
  * [Vuex](#vuex)
- [SPA-SEO Problem](#spa-seo-problem)
- [Examples](#examples)
  * [Example 1](#example-1)
  * [Example 2](#example-2)
  * [Example 3](#example-3)
  * [Example 4](#example-4)
- [Resources](#resources)

<!-- tocstop -->

# Vue CLI and Vue GUI

Vue için CLI ve GUI olmak üzere aynı işi farklı arayüzlerle yapan 2 farklı yardımız var. Aslında bir Vue projesi başlatmak için bu ikisinede gerek yok ancak birkaç tıkla tüm projemizin iskeletini oluşturmak için bunları kullanıyoruz.

Örneğin bu araçların bir faydası `Webpack` ayarı yapmaktan bizi kurtarması. Webpack nedir? Proje derleme aşamasında kodumuzu `minify` eden yapı. İnsanlar bunu ayarlamanın zor olduğunu söylüyor. Gördüğüm kadarıyla isteğe göre linter ve prettier ayarlıyor, bu ve bunun gibi şeyleri projeye implemente etmek yorucu olduğu için en mantıklı seçenek bu araçların kullanılması.

# File Structure

Kök dizinde `main.js` adındaki dosya `App.vue` dosyasını alıp `index.html` içine yazdığımız kodu webpack ile geçirip bundle haline getirdikten sonra inject ediyor. Sonrasında `dist` adında bir klasörümüz oluşuyor ve artık bu klasörü `deploy` edebiliyoruz.

`main.js` içine `vue-router` ve `veux` kütüphanelerinden `route.js` ve `store.js` tarzında yapıları tanıtmak için kullanıyoruz. Bunun dışında `main.js` dosyasına dokunmamamız gerekiyor.

Aslında yazdığımız bütün komponenetler bir şekilde `App.vue` dosyası içine kadar ulaşıyor.

# Basic Logic

Vue fonksiyonu ile bir DOM elementine bağlantı kuruyoruz, fonksiyona verdiğimiz parametreler ile dom elementini yönetiyoruz. `Expression` ile DOM elementi üstünde verielri gösterebiliyoruz.

## Expressions

View içinde bu şekilde verileri göstermemiz mümkün.

```
{{ product }}
{{ count += 1 }}
{{ firstName + ' ' + lastName }}
{{ message.split('').reverse().join('') }}
{{ isWorking ? 'YES' : 'NO' }}
```

# Testing

Yazdığımız kodları test etmek için Javascript kütüphaneleri mevcut. `Mocha` ve `Jest` gibi. Vue CLI ile proje yaratırken bize soruyor ne seçebileceğimizi. Nasıl test yazılır bilmediğim ve bana advanced geldiği için bu konuyu şimdilik atlıyorum.

# Features

Vue.js ile gelen bazı özellikler ve detayları.

## Single Page Component

Vue ile tek sayfa içinde html, js ve css kullanarak elementler geliştirip projemizi parçalar halinde geliştirebiliyoruz. Kodumuz `template`, `script`, `style` olarak 3 kısma ayrılıyor.

## Two Way Data Bindings

Model ve View arasındaki tüm değişimler hiç bir method vs kullanmaksızın birbirine işliyor.

`v-model` kullanıyoruz bunun için.

Normalde elemenete method tanımlayıp method sonucu çıkan datayı elementler arası göndermem gerekiyordu, data binding ile bu iş yoyutlanmış oluyor ve daha kolay şekilde verileri dağıtabiliyorum.

Normalde getter, setter methodları oluyor. Sanırım bu özellik ile getter, setter ihtiyacı ortadan kaldırılmış.

## Attribute Binding

HTML sayfası içinde bir resmi değiştirmek istedik diyelim, o zaman image elementinin `src` özelliğini değiştirmemiz gerekir. Vue ile image etiketi içinde `v-bind:src="image"` kullanarak image değişkenine atılan değer image etiketinin src özelliğine etki edecek.

Aslında burada image diye belirttiğimiz değer bir expression. Yani değişken yerine expression bir ifade kullanabilirdik.

Bu özellik çok kullanıldığı için bir kısayolu mevcut. Örneğin `v-bind:src` yerine `:src` şeklinde yazabiliyoruz.

## Conditional Rendering

`v-if`, `v-else-if` ve `v-else` kullanarak duruma bağlı olarak ekrana element renderlama işlemi.

```html
<p v-if="inventory > 100"in Stock</p>
<p v-else-if="inventory <= 10 && inventory > 0">Almost sold out!</p>
<p v-else>Out of Stock</p>
```

Bu kullanımda element ya kaldırılır ya eklenir. Gizlenmez.

Eğer çok daha hızlı ve basit bir şekilde bir elementi göstermek yada gizlemek istiyorsak `v-show` kullanabiliriz.

```html
<p v-show="inStock">in Stock</p>
```

Bu şekilde kullandığımızda element'in `CSS-display` özelliği `none` yapılıyor ve bu şekilde gizleniyor.

## List Rendering

```html
<ul>
  <li v-for="detail in details">
    {{ detail }}
  </li>
</ul>
```

Liste halindeki objeleri yazıdırmak

```html
<div v-for="variant in variants" :key="variant.variantId">
  <p>{{ variant.color }}</p>
</div>
```

Görüldüğü üzere liste şeklinde yazdığımız için elemente `v-bind` ile uniq bir id atadık çünkü liste şeklinde element oluşturunca id atamak tavsiye ediliyor.

```html
<ul>
  <li v-for="(detail, index) in details">
    {{ index }} {{ detail }}
  </li>
</ul>
```

Bu şekilde for döngüsündeki elemetleri numaralandırmak mümkün.

## Event Handling

```html
<div id="app">
  <button v-on:click="count += 1">Increase</button>
  <span>Count is {{ count }}</span>
</div>
```

Bu doğru bir kullanım ancak proje büyüdükçe bu gibi durumları daha rahat ele almak için method içinde belirtmek daha akıllıca bir çözüm.

```html
<div id="app">
  <button v-on:click="increaseCount">Increase</button>
  <span>Count is {{ count }}</span>
</div>

<script>
  var app = new Vue({
    el: '#app',
    data: {
      count: 0
    },
    methods: {
      increaseCount: function() {
        this.count += 1;
      }
    }
  });
</script>
```

`v-on:click` yerine kısaca `@click` kullanabiliriz. Bu iki kullanım arasında hiçbir fark yok.

Örnek bazı eventler: 

- `@mouseover`
- `@click`
- `@submit` (form elementi için)
- `@keyup.enter` (Input elementinde enter tuşuna basıp elimizi çektikten sonra çalışan event)

Method tanımlaması yaparken `methodName: function(){...}` yerine `methodName(){...}` şeklinde yazabilirdik ancak bazı tarayıcılar bunu desteklemediği için ilk yazdığım kod tarzı tercih ediliyor.

## Class & Style Binding

```html
<div id="app">
  <div class="container" :style="{ backgroundColor: color, fontSize: size }"></div>
  <div :style="styleObject"></div>
</div>

<script>
  var app = new Vue({
    ul: '#app',
    data: {
      color: 'green',
      size: '13px',
      styleObject: {
        fontSize: '15px'
      }
    }
  });
</script>
```

Bu ifade render edildiğinde şuna dönüşür;

```html
<div class="container" style="{ background-color: green; font-size: 13px }"></div>
<div style="{ font-size: 15px }"></div>
```

- `backgroundColor` gibi bir ifade yerine `'backgorund-color'` şeklinde değer belirtmemiz mümkün.
- Örnekte görüldüğü üzere stil objesi yaratıp  tanımlamamız mümkün.
- Birden fazla stil objesi tanımlamak istiyorsak `[styleObject1, styleObject2]` şeklinde array içinde tanımlamamız mümkün.

```html
<button class="{disabledButton: !inStock}" :disabled="!inStock">Karta Ekle</button>
```

Yukarıdaki örnekte inStock değeri false ise butonun üstüne disabledButton class'ı uygulanıyor.

```html
<div class="container" :class="{active: activeClass, 'text-danger': errorClass}"></div>
```

Yukarıdaki örnekte varsayılan olarak container adlı bir sınıf uygulanmış elemente. Elimizdeki activeClass ve errorClass'a bakarak 2 farklı class daha ekleyebiliriz. Veri'nin işleyişine göre elemente sınıf ekliyoruz kısaca. Örneğin activeClass değeri true ve errorClass değeri false olursa eğer ortaya aşağıdaki çıktı render edilir.

```html
<div class="container active"></div>
```

Sınıflar için stillerde olduğu gibi objeler tanımlamak mümkün. Aynı şekilde array olarak class objeleri tanımlamamız mümkün.

```html
<div :class="[isActive ? activeClass : '']"></div>

<!--
data: {
  isActive: true,
  activeClass: 'active'
}
-->
```

Yukarıdaki şekilde olduğu gibi class tanımlamak ta mümkün. Çıktısı aşağıdaki gibi olacaktır;

```html
<div class="active"></div>
```

## Computed Properties

```html
<div id="app">
  <p>{{ title }}</p>
</div>

<script>
  var app = new Vue({
      ul: '#app',
      data: {
        name: 'Boat',
        brand: 'Lip',
        color: 'Blue'
      },
      computed: {
        title() {
          return this.brand + ' ' + this.color + ' ' + this.name
        }
      }
  }); 
</script>
```

Görüldüğü üzere bu şekilde elimizdeki verileri daha basit bir yapıda elemente yerleştirebiliyoruz. Bu yapının kullanılma amacı elimizdeki veriyi işleyip tek bir veri şeklinde döndürebilmek, örnekte 3 string veriyi birleştirip tek elementte daha rahat kullanabileceğimiz bir yapı tasarlamış olduk.

## Components

Program geliştirirken işimizi kolaylaştırması için kodları mantıklı parçalara bölüyoruz. Bu parçaları lego olarak düşünebiliriz. Vue ile de aynı şekilde tasarımlar yapıyoruz. Bu şekilde modüler bir proje üretmiş oluyoruz, geliştirmesi ve okunması daha kolay kod yazdığımız için komponenet yapısını öğrenmemiz gerekiyor.

```html
Vue.component(name, {});
```

# data() vs data:

```javascript
new Vue({
  el: "#app",
  data: {
      message: 'hello world!'
    }

});

new Vue({
  el: "#app",
  data: function() {
    return{
      message: 'hello world!'
    }
  }

});

new Vue({
  el: "#app",
  data() {
    return {
      message: 'hello world!'
    }
  }
});
```

Vue methodu tanımlanırken ES6'da kullanılan `arrow function`'lar  kullanılmaz çünkü `this` yapısı sorun çıkartıyor. Vur dokümantasyonunda bu durum belirtilmiş. 

ES6 yapısında fonksiyon `data()` şeklinde tanımlanırken ES5 yapısında `data:` şeklinde tanımlanıyor.

# Libraries

Vue ile proje geliştirirken işimize yarayabilecek bazı temel kütüphaneler.

## Fetch API

API'lardan data çekmek için bunu kullanabiliyoruz. Vue orjinal dokümantosyonunda `axios` kullanın diyor.

```javascript
fetch('http://example.com/movies.json')
  .then(function(response) {
    return response.json();
  })
  .then(function(myJson) {
    console.log(JSON.stringify(myJson));
  });
  .catch(error => console.error('Error:', error));
```

## Axios

`fetch` yerine kullanabileceğimiz bir kütüphane.

```javascript
axios
  .get('https://api.coindesk.com/v1/bpi/currentprice.json')
  .then(response => (this.info = response))
  .catch(error => console.log(error))
  .finally(() => this.loading = false)
```

## Vue Router

Uygulamayı `Single Page Application` yaparken en büyük yardımcımız bu kütüphane.

## Vuex

React ta Redux neyse Vue'de Veux o. State managment kütüphanesi. Verileri yönetmemizi kolaylaştıran bir kütüphane kısaca. Neden böyle bir şeye ihtiyacımız var? Uygulamamızı parçalara bölüp kodladığımız için ve farklı parçalarda aynı verilere bazı zamanlar ihtiyaç olduğu için kullanıyoruz.

- State
- Getters
- Mutations
- Modules
- Actions

Bu başlıklar vuex'in yapı taşları.

- Her mutation yanında bir action olmalı, mutation'lar tek başına kullanılmıyorlar.

- Mutation her zaman senkron çalışmak zorudna, asenkron işler çağıramayız fetch gibi, yada delay koyamayız. Bu tür bir işlem için action kullanabiliriz, actionlar asenkron olabilirler.

- İstersek aradan action'ı çıkartabilriiz. Bunu yapmak için mapMutations çağırıp action gibi kullanabiliriz.Dokümanalrda bu kullanım tavsiye edilmiyor.

- Modules yapısıyla vuex storeları birbirinden soyutlanabiliyor.

```javascript
const store = new Vuex.Store({
  modules: {
    user: {
      state,
      getters,
      mutations,
      actions 
    },
    card: {
      state2,
      getters2,
      mutations2,
      actions2
    }
  }
});
```

Vue komponent state ile oluşturulur. komponent bir action çalıştırıyor, action mutation çalıştırıyor ve state değiştiriliyor bu şekilde ynei komponent oluşturuluyor. Bu şekilde bir döngü mevcut. State yapısı ile komponent oluşturup komponent action->mutaition-->state zinciri ile güncellenmesini sağlayabiliyoruz.

src klasörü altında store adıdna bir klasör açıp index.js adında bir dosya ekle.

```javascript
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const state = {
  username: 'ridvanaltun',
  message: 'Hello!'
};

const getters = {
  welcomeMessage(state) {
    return '${state.message} ${state.username}';
  },
};

const mutations = {
  setUserName(state, name) {
    state.username = name;
  }
};

const actions = {
  updateUserName({ commit }, name) {
    commit('setUserName', name);
  }
};

const store = new Vuex.Store({
  state,
  getters,
  mutations,
  actions
});

export default store;
```

```javascript
import Vue from 'vue';
import App from './App.vue';
import store from './src/store/index';

new Vue({
  el: '#app',
  render: h => h(App),
});
```

```html
// App.vue

import { mapState, mapGetters, mapActions } from 'vuex';

<script>
  export default {
    name: 'App',
    data(
      return {
        name: '',
      };
    ),
    computed: {
      ...mapState([
        'username',
        'message',
      ]),
      ...mapGetters([
        welcomeMessage,
      ]),
    },
    methods: {
      ...mapActions([
        'updateUserName'
      ]),
      updateName() {
        this.updateName(this.name);
      },
    }
  }
</script>

<template>
  <div id="app">
    {{message}} {{username}}
    {{welcomeMessage}}

    <input v-model="name" />
    <button @onclick="updateName">Update Name</button>
  </div>
</template>

<style>
#app {
  margin-top: 60px; 
}
</style>
```

# SPA-SEO Problem

Bu konu Vue özelidne değil ancak hatırlamakta fayfa var. `Single Page Application` yaparken SEO'ya dikkat etmek gerekiyor. Örneğin e-ticaret sitesi yaparken tamamını SPA uyumlu yaparsak muhtemelen şirketi batırız çünkü google botları sitenin kaynak kodlarını göremediği için site sıralamada çıkmayacaktır.

Emin değilim ama bunun: [vue-meta](https://github.com/nuxt/vue-meta) gibi çözümler üretilmiş.

# Examples

Öğrenmenin en iyi yolu pratikle desteklemek.

## Example 1

```html
<div id="app">
    <h1>{{ product }} in stock.</h1>
</div>
<script src="https://unpkg.com/vue"></script> 
<script>
    const app = new Vue({
        el: '#app',
        data: {
            product: 'Boots'
        }
    });
</script>
```

## Example 2

```html
<div id="app">
    <ul>
        <li v-for="product in products">
        {{ product }}
        </li>
    </ul>
</div>
<script src="https://unpkg.com/vue"></script> 
<script>
    const app = new Vue({
        el: '#app',
        data: {
            products: [
                'Boots',
                'Phones',
                'Pens'
            ]
        }
    });
</script>
```

## Example 3

```html
<div id="app">
    <ul>
        <li v-for="employee in employees">
        {{ employee.name }}
        </li>
    </ul>
</div>
<script src="https://unpkg.com/vue"></script> 
<script>
    const app = new Vue({
        el: '#app',
        data: {
            employees: []
        },
        created () {
            fetch('http://dummy.restapiexample.com/api/v1/employees')
                .then(response => response.json())
                .then(json => {
                    this.employees = json 
                })
        }
    });
</script>
```

## Example 4

```html
<div id="app">
  <ul>
    <li v-for="employee in employees">
    <input type="number" v-model.number="employee.employee_age"> - {{ employee.employee_name }}
    <span v-if="employee.employee_age == 23">
      -- Age 23
    </span>
    <button @click="employee.employee_age = parseInt(employee.employee_age) + 1">
      +1 Age
    </button>
    </li>
  </ul>
  <h2>Employee Age Sum: {{ employeeAgeSum }}</h2>
  <h2>Employee Count: {{ employeeCount }}</h2>
</div>
<script src="https://unpkg.com/vue"></script> 
<script>
const app = new Vue({
  el: '#app',
  data: {
    employees: []
  },
  computed: {
    employeeAgeSum() {
      return this.employees.reduce((sum, employee) => { return sum + parseInt(employee.employee_age)}, 0)
    },
    employeeCount() {
      return this.employees.length;
    }
  },
  created () {
    fetch('http://dummy.restapiexample.com/api/v1/employees')
      .then(response => response.json())
      .then(json => {
        this.employees = json; 
      })
  }
});
</script>
```

# Resources

- [Intro to Vue.js](https://www.vuemastery.com/courses/intro-to-vue-js), Vue.js temelleri çok güzel bir şekilde anlatılıyor.

- [Real World Vue.js](https://www.vuemastery.com/courses/real-world-vue-js), temelleri aldıktan sonra burayı takip edebilirsiniz.