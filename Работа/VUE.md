# VUE

[VUEX](VUEX%205e2f67042d4d4c09a7cf338babd5dc7a.md)

## Содержание

## [Table](VUE.md)

- [Как настроить шаблон внутри ячейки таблицы(вверх)](VUE.md)
- [как прописать в цикле темплейты по полям](VUE.md)

## Корневой компонент с перебором ссылок в цикле[(вверх)](VUE.md)

```jsx
<template>
  <div class="container">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <ul class="navbar-nav">
          <li v-for="route in $router.options.routes" :class="{active: $route.path == route.path}" class="nav-item">
            <router-link :to="route.path" class="nav-link" :key="route.path">
              {{ route.meta.title }}
            </router-link>
          </li>
        </ul>
      </div>
    </nav>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
name: "VTracker",
}
</script>

<style scoped>

</styl
```

Вместо<router-view></router-view> появится тот компонент, на который будет сделан переход либо через link, либо через router.push(‘/path/rightPath’)

## Корневой элемент с прописанными роутами[(верх)](VUE.md)

```jsx
 <template>
  <div>
    <div class="d-flex flex-row">
      <router-link to="'admin/eft/ban/ragfair/addRagFairBan'" class="nav-link">Добавить</router-link>
      <router-link to="'admin/eft/ban/ragfair'" class="nav-link">Список банов</router-link>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
name: "RagFairBan"
}
</script>

<style scoped>

</style>
```

## Корневой элемент с пропсами[(вверх)](VUE.md)

```jsx
<template>
  <div class="container-fluid pl-3">
    <ScreenLocker/>
    <router-link
        v-if="$route.path != '/admin/locations/lootGenerator'"
        :to="'/admin/locations/lootGenerator'"
        class="nav-link">
      Locations</router-link>
    <div class="m-2 h2">{{ path }}</div>
    <div v-if="$route.path != '/admin/locations/lootGenerator'">
      <div class="h2 mb-3">

      </div>
      <div class=" d-flex flex-column">
        <div class="d-flex flex-row">
        <router-link :to="`/admin/locations/lootGenerator/byCategory/${idLocation.id}`" class="nav-link">
          по категориям
        </router-link>
        <router-link :to="`/admin/locations/lootGenerator/byPoints/${idLocation.id}`" class="nav-link">
          по точкам
        </router-link>
        <router-link :to="`/admin/locations/lootGenerator/byJson/${idLocation.id}`" class="nav-link">
          JSON
        </router-link>
        </div>
      </div>
    </div>
    <router-view></router-view>
  </div>

</template>

<script>

import ScreenLocker from "../../CommonComponent/ScreenLocker";
export default {
name: "LootGenerator",
  components: {ScreenLocker},
  computed: {
    idLocation() {
      return this.$store.getters['lootGeneratorStore/getSelectedLocation'];
    },
    path() {
      return (this.idLocation == "") ? 'Локации' : 'Локации' + ' -> ' + this.idLocation.name + '(' + this.$route.meta.title + ')';
    }
  },
}
</script>

<style scoped>

</style>
```

## файл JS[(вверх)](VUE.md)

```jsx
import Vue from 'vue';
import VueAxios from 'vue-axios';
import VueRouter from 'vue-router';
import store from '../../store/store';
import BootstrapVue from 'bootstrap-vue';
import axios from "axios";

const Course = () => import(/* webpackChunkName: "course/Course" */'./Course.vue');
const AddCourse = () => import(/* webpackChunkName: "course/AddCourse" */'./Components/AddCourse');
const EditCourse = () => import(/* webpackChunkName: "course/EditCourse" */'./Components/EditCourse');
const ViewCourses = () => import(/* webpackChunkName: "course/ViewCourse" */'./Components/ViewCourses');

Vue.use(VueRouter);
Vue.use(BootstrapVue);
Vue.use(VueAxios, axios);

const router = new VueRouter({
    mode: 'history',
    routes: [
        {path: '/admin/eft/course', component: ViewCourses},
        {path: '/admin/eft/course/addCourse', component: AddCourse},
        {path: '/admin/eft/course/editCourse/:id', component: EditCourse, props: true },
    ],
});

const app = new Vue({
    el: "#courseApp",
    store,
    router,
    render: h => h(Course),
});
```

## Передача значений в роутере[(вверх)](VUE.md)

```jsx
import Vue from 'vue'
import VueRouter from 'vue-router'
import Hello from './Hello.vue'

Vue.use(VueRouter)

function dynamicPropsFn (route) {
 const now = new Date()
 return {
   name: (now.getFullYear() + parseInt(route.params.years)) + '!'
 }
}
const router = new VueRouter({
 mode: 'history',
 base: __dirname,
 routes: [
   { path: '/', component: Hello }, // No props, no nothing
   { path: '/hello/:name', component: Hello, props: true }, // Pass route.params to props
   { path: '/static', component: Hello, props: { name: 'world' }}, // static values
   { path: '/dynamic/:years', component: Hello, props: dynamicPropsFn }, // custom logic for mapping between route and props
   { path: '/attrs', component: Hello, props: { name: 'attrs' }}
 ]
})

new Vue({
 router,
 template: `
   <div id="app">
     <h1>Route props</h1>
     <ul>
       <li><router-link to="/">/</router-link></li>
       <li><router-link to="/hello/you">/hello/you</router-link></li>
       <li><router-link to="/static">/static</router-link></li>
       <li><router-link to="/dynamic/1">/dynamic/1</router-link></li>
       <li><router-link to="/attrs">/attrs</router-link></li>
     </ul>
     <router-view class="view" foo="123"></router-view>
   </div>
 `
}).$mount('#app')
```

## Файл store[(вверх)](VUE.md)

```jsx
 const state = {
    versions: [],
};
const getters = {
    getMajorVersions(state) {
        return state.versions;
    },
};
const actions = {
    loadMajorVersions({dispatch, commit}) {
        return dispatch("apiCallGET",
            {
                url: '/admin/vtracker/major/api/loadMajorVersions',
                parameters: {}
            }, {root: true}
        )
            .then(
                response => {
                    commit("setMajorVersions", response);
                }
            );
    },
    addMajorVersion({dispatch, commit}, parameters) {
        return dispatch("apiCallPOST",
            {
                url: '/admin/vtracker/major/api/addMajorVersion',
                parameters: parameters,
            }, {root: true}
        )
            .then(
                response => {
                    dispatch('loadMajorVersions');
                    return response;
                }
            );
    },
    deleteMajorVersion({dispatch, commit}, parameters) {
        return dispatch("apiCallPOST",
            {
                url: '/admin/vtracker/major/api/deleteMajorVersion',
                parameters: parameters,
            }, {root: true}
        )
            .then(
                response => {
                    dispatch('loadMajorVersions');
                    return response;
                }
            );
    },
    setEnableLogin({dispatch, commit}, parameters) {
        return dispatch("apiCallPOST",
            {
                url: '/admin/vtracker/major/api/setEnableLogin',
                parameters: parameters,
            }, {root: true}
        )
            .then(
                response => {
                    dispatch('loadMajorVersions');
                    return response;
                }
            );
    },
    setNotPublicBuild({dispatch, commit}, parameters) {
        return dispatch("apiCallPOST",
            {
                url: '/admin/vtracker/major/api/setNotPublicBuild',
                parameters: parameters,
            }, {root: true}
        )
            .then(
                response => {
                    dispatch('loadMajorVersions');
                    return response;
                }
            );
    },
    setAlwaysActual({dispatch, commit}, parameters) {
        return dispatch("apiCallPOST",
            {
                url: '/admin/vtracker/major/api/setAlwaysActual',
                parameters: parameters,
            }, {root: true}
        )
            .then(
                response => {
                    dispatch('loadMajorVersions');
                    return response;
                }
            );
    }
};

const mutations = {
    setMajorVersions(state, versions) {
        state.versions = versions;
    }
};

export default {
    namespaced: true,
    state,
    getters,
    actions,
    mutations,
}
```

## Класс контейнеры[(вверх)](VUE.md)

**class = “container-fluid”** на всю ширину

**class = “container”** по центру

**class="text-center"** текст по центру

**class="w-100"** растянут размер на всю ширину

**class="d-flex flex-row"** флекс контейнер с расп. доч. элем. в строку

**class="d-flex flex-column w-75 align-items-center"** флекс контейнер с расп. доч. элем. в колонку и выровненных по вертикали по центру

**class="d-flex flex-row justify-content-between w-100"** флекс контейнер с расп. доч. элем. в строку на всю ширину с расположением компонентов на равном максимальном друг от друга расстоянии

## Чанки[(вверх)](VUE.md)

```jsx
const CurrentRagFairBan = () => import(/* webpackChunckName: "RagFairBan/currentRagFairBan"*/'./Components/CurrentRagFairBan.vue');
```

## Настройка Роутеров с историей, передачей пропсов и мета[(верх)](VUE.md)

```jsx

const router = new VueRouter({
    mode:'history',
    routes: [
        {path: '/admin/locations/lootGenerator/', component: LootLocations},
        {path: '/admin/locations/lootGenerator/byCategory/:idLocation', component: LootGenerateByCategory, meta: {title: 'по категориям'}, props: true},
        {path: '/admin/locations/lootGenerator/byPoints/:idLocation', component: LootGenerateByPoints, meta: {title: 'по точкам'}, props: true},
        {path: '/admin/locations/lootGenerator/byJson/:idLocation', component: LootGenerateJson, meta: {title: 'JSON'}, props: true},

    ],
});
```

## Регулярные выражения[(вверх)](VUE.md)

[Спецсимволы](Спецсимволы%2057a5e4ffdb3240ecab0bd3ae4b5abb24.csv)

[Спецсимволы внутри символьного класса](Спецсимволы%20внутри%20символьного%20класса%20eb4212b7d72547e6b460350e09e74313.csv)

[Позиция внутри строки](Позиция%20внутри%20строки%20e6a996bbd56f45ea953c6caaeaa8478c.csv)

- **Инструкция по созданию регулярного выражения**
    
    Для составления регулярных выражений можно воспользоваться [онлайн сервисом regex101.com](https://regex101.com/). В нем можно сразу проверить любой набор слов составляемой регуляркой и есть описание элементов, используемых в регулярках.
    
    Важно не забывать, что символы **^ $ * + ? { } [ ] \ | ( )** нужно экранировать с помощью **\** перед символом. В нашем случае нет необходимости беспокоиться о проверки слов в разном регистре, все слова, включая ники игроков, приводятся в нижний регистр при проверке
    
    Предположим, мы хотим сделать регулярку, которая находит междометия, обозначающие смех. Просто «Хаха» нам не подойдёт — ведь под него не попадут «Хехе», «Хохо» и «Хихи».
    
    Конечно, мы можем создать три регулярки на каждый случай. Но правильнее будет использовать набор. Вместо указания конкретного символа - записать целый список, и если в исследуемой строке на указанном месте будет стоять любой из перечисленных символов, строка будет считаться подходящей. Наборы записываются в квадратных скобках — паттерну [abcd] будет соответствовать любой из символов «a», «b», «c» или «d».
    
    Итак, применяя данный инструмент к нашему случаю, если мы напишем [Хх][аоие]х[аоие], то каждая из строк «Хаха», «хехе», «хихи» и «Хохо» будут соответствовать шаблону.
    
    Допустим, нам нужно регулярное выражение, для поиска слова **nigger**. Кроме него, мы догадываемся, что есть целый список возможных однокоренных слов, например: nigga, nigger, niga. Также, они могут быть записаны с заменой i на 1 и g на 9. Постараемся составить регулярное выражение, которое будет покрывать большое количество вариантов, например: nigger, niger, ni99er, n199er, nigga, n1gga, niga, n1ga, n19a. Мы можем сразу добавить эти слова в раздел Test Srtring сервиса regex101, а затем собрать регулярное выражение.
    
    Первый символ n должен оставаться в слове всегда, а за ним должно быть что-то из набора [i1]. После этого идет набор из g и 9. Запишем его как [g9], затем укажем, что таких симвлов может быть 1 или два с помощью {1,2} После этого в слове идет символ из набора [ae]. Итоговое выражение получается: n[i1][g9]{1,2}[ae]. Обратите внимание, что справа в разделе Explanation сервис дает поясления к каждой из составных частей используемого выражения.
    
    Приведем еще один пример. Допустим, нам надо забанить ники, которые пытаются использовать слово **hitler**. В этом случае помимо замены i на 1, могут быть написания где l заменяется на 1, e заменяется 3, а h - x и r - p соответственно. Набор запрещенных ников может выглядеть вот так: hitler, h1tl3r, h1t13p. Также мы можем захотеть забанить сокращения и варианты где использовано сразу несколько одинаковых букв подряд: hitla, hittttttla, h1ttleeeeer, hhhitler, hhh1t13р, xxxitl3r, x1t13p. Итоговым вариантом регулярного выражения может быть: [hx][i1]+t+[l1]+[e3a]+Она ищет шаблон, начинающийся с h или x, после которого должно быть больше 1 символа из набора i1, затем не менее одной t, не менее одного символа l или 1 и наконец что-то из набора [e3a] так же не менее одного раза.
    
    При составлении регулярных выражений важно чтобы оно с одной стороны охватывала максимум случаев запрещенных слов, а с другой - не находила запрещенные корни в нормальных словах. Например, можно забанить слово **petuh** с помощью разных регулярок. Но при этом важно понимать, что корень petu содержится в ряде нормальных слов, например petunia - это название растения. Большое количесто вариантов написания слова: petuh, petux, p3tux, p3tuh, peeetuxx, p33etuhh можно найти с помощью выражения p[e3]+t+[uy]+[hx]+, которое однако пропустит разрешенные однокоренные слова
    
    **Важно:**
    
    Внутри набора большая часть спецсимволов не нуждается в экранировании, однако использование \ перед ними не будет считаться ошибкой. По прежнему необходимо экранировать символы «\» и «^», и, желательно, «]» (так, [][] обозначает любой из символов «]» или «[», тогда как [[]х] – исключительно последовательность «[х]»). Необычное на первый взгляд поведение регулярок с символом «]» на самом деле определяется известными правилами, но гораздо легче просто экранировать этот символ, чем их запоминать. Кроме этого, экранировать нужно символ «-», он используется для задания диапазонов (см. ниже).
    
    Если сразу после [ записать символ ^, то набор приобретёт обратный смысл — подходящим будет считаться любой символ кроме указанных. Так, паттерну [^xyz] соответствует любой символ, кроме, собственно, «x», «y» или «z».
    
    ```jsx
    Можно через авто-замену в шторме сделать:
    slot="(.*)" slot-scope="(.*)"
     - и нажать в поиске кнопку .*
    #cell($1)="$2"
    
    Если где-то имена слотов использовали в цикле, например:
    <template v-for="(lang) in languageList" v-model="languageList" :slot="lang.label" slot-scope="row">
    
    Можно заменить на:
    <template v-for="(lang) in languageList" v-model="languageList" v-slot:[`cell(${lang.label})`]="row">
    
    Если где-то меняли в таблицах хедеры/футеры, новый синтаксис:
    slot="HEAD_(.*)" slot-scope="(.*)"
    #head($1)="$2"
    
    slot="FOOT_(.*)" slot-scope="(.*)"
    #foot($1)="$2"
    
    Для слота busy в таблице:
    v-slot:table-busy
    #table-busy
    
    Слот деталей:
    <template v-slot:row-details="row">
     <template #row-details="row">
    ```
    
    ![Untitled](Работа/VUE/Untitled.png)
    

![Untitled](Работа/VUE/Untitled%201.png)

### Якоря

Якоря в регулярных выражениях указывают на начало или конец чего-либо. Например, строки или слова. Они представлены определенными символами. К примеру, шаблон, соответствующий строке, начинающейся с цифры, должен иметь следующий вид:

```html
^[0-9]+
```

Здесь символ ^обозначает начало строки. Без него шаблон соответствовал бы любой строке, содержащей цифру.

### Символьные классы

Символьные классы в регулярных выражениях соответствуют сразу некоторому набору символов. Например, \dсоответствует любой цифре от 0 до 9 включительно, \wсоответствует буквам и цифрам, а \W— всем символам, кроме букв и цифр. Шаблон, идентифицирующий буквы, цифры и пробел, выглядит так:

```html
\w\s
```

### Утверждения

Поначалу практически у всех возникают трудности с пониманием утверждений, однако познакомившись с ними ближе, вы будете использовать их довольно часто. Утверждения предоставляют способ сказать: «я хочу найти в этом документе каждое слово, включающее букву “q”, за которой не следует “werty”».

```html
[^\s]*q(?!werty)[^\s]*
```

Приведенный выше код начинается с поиска любых символов, кроме пробела (`[^\s]*`), за которыми следует
`q`. Затем парсер достигает «смотрящего вперед» утверждения. Это автоматически делает предшествующий
элемент (символ, группу или символьный класс) условным — он будет соответствовать шаблону, только если
утверждение верно. В нашем случае, утверждение является отрицательным (`?!`), т. е. оно будет верным,
если то, что в нем ищется, не будет найдено.

Итак, парсер проверяет несколько следующих символов по предложенному шаблону (`werty`). Если они найдены,
то утверждение ложно, а значит символ `q`будет «проигнорирован», т. е. не будет соответствовать шаблону.
Если же `werty`не найдено, то утверждение верно, и с `q`все в порядке. Затем продолжается
поиск любых символов, кроме пробела (`[^\s]*`).

Примеры регулярок

Ищет строку, начинающуюся с Kernel\Modules и строку, которая не начинается с Kernel\Modules

```jsx
^(?:Kernel\\Modules).+
^(?!Kernel\\Modules).+
```

```jsx
^(?:modules).+
^(?!namespace Kernel\\Modules).+
```

## Стили BootStrap[(вверх)](VUE.md)

### Текст разного цвета

![VUE%20dae91e89c47648eaad4a575c11514166/Untitled%202.png](Работа/VUE/Untitled%202.png)

```html
<p class="text-primary">.text-primary</p>
<p class="text-secondary">.text-secondary</p>
<p class="text-success">.text-success</p>
<p class="text-danger">.text-danger</p>
<p class="text-warning">.text-warning</p>
<p class="text-info">.text-info</p>
<p class="text-light bg-dark">.text-light</p>
<p class="text-dark">.text-dark</p>
<p class="text-muted">.text-muted</p>
<p class="text-white bg-dark">.text-white</p>
```

### Класс контейнеры[(вверх)](VUE.md)

на всю ширину

```jsx
**class = “container-fluid"**
```

по центру

```jsx
**class = “container”**
```

 текст по центру

```jsx
class="text-center"**
```

растянут размер на всю ширину

```jsx
class="w-100"
```

флекс контейнер с расп. доч. элем. в строку

```jsx
class="d-flex flex-row" 
```

флекс контейнер с расп. доч. элем. в колонку и выровненных по вертикали по центру

```jsx
class="d-flex flex-column w-75 align-items-center"
```

флекс контейнер с расп. доч. элем. в строку на всю ширину с расположением компонентов на равном максимальном друг от друга расстоянии

<aside>
📎 class="d-flex flex-row justify-content-between w-100"

</aside>

### input тег был только численным[(вверх)](VUE.md)

<aside>
📎 type="number"

</aside>

Пример

```php
<input type="number" id="tentacles" name="tentacles"
min="10" max="100">
```

## v-for[(вверх)](VUE.md)

- **Принимает:** `Array | Object | number | string | Iterable (с версии 2.6)`
- **Использование:**
    
    Многократно отрисовывает элемент или блок шаблона, основываясь на переданных данных. Значение директивы должно следовать синтаксису `alias in expression` — в `alias` будет элемент текущей итерации:
    
    ```
    <div v-for="item in items">
      {{ item.text }}
    </div>[(вверх)](VUE%20dae91e89c47648eaad4a575c11514166.md)
    ```
    
    Кроме того, вы можете указать название для индекса (или ключа, если вы работаете с объектом):
    
    ```
    <div v-for="(item, index) in items"></div>
    <div v-for="(val, key) in object"></div>
    <div v-for="(val, name, index) in object"></div>
    ```
    
    По умолчанию `v-for` будет пытаться обновить элементы «на месте», не перемещая их. Если вам нужно, чтобы элементы перемещались, сохраняя явную упорядоченность, укажите атрибут `key`:
    
    ```
    <div v-for="item in items" :key="item.id">
      {{ item.text }}
    </div>
    ```
    
    С версии 2.6.0+, `v-for` также может работать со значениями, реализующими [**протокол Iterable**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterable_protocol), включая нативные `Map` и `Set`. К сожалению, Vue 2.x в настоящее время не поддерживает реактивность для значений `Map` и `Set` и поэтому не сможет автоматически отслеживать изменения в них.
    
    При совместном использовании `v-if` и `v-for`, `v-for` имеет более высокий приоритет. Подробности на странице [**отрисовки списков**](https://ru.vuejs.org/v2/guide/list.html#v-for-%D0%B8-v-if%22).
    
    Использование `v-for` подробно описано в секции руководства по ссылке ниже.
    
- **См. также:**
    - [**Отрисовка списков**](https://ru.vuejs.org/v2/guide/list.html)
    - [**key**](https://ru.vuejs.org/v2/guide/list.html#key)
    

## v-on[(верх)](VUE.md)

- **Сокращение:** `@`
- **Принимает:** `Function | Inline-выражение | Object`
- **Параметр:** `event`
- **Модификаторы:**
    - `.stop` — вызовет `event.stopPropagation()`.
    - `.prevent` — вызовет `event.preventDefault()`.
    - `.capture` — добавит подписку в режиме capture.
    - `.self` — вызовет обработчик только если событие возникло непосредственно на этом элементе.
    - `.{keyCode | keyAlias}` — вызывает обработчик только при нажатии определённой клавиши.
    - `.native` — подписаться на нативное событие на корневом элементе компонента.
    - `.once` — вызовет обработчик не больше одного раза.
    - `.left` - (2.2.0) вызов обработчика только по событию нажатия левой кнопки мыши.
    - `.right` - (2.2.0) вызов обработчика только по событию нажатия правой кнопки мыши.
    - `.middle` - (2.2.0) вызов обработчика только по событию нажатия средней кнопки мыши.
    - `.passive` - (2.3.0+) вызов обработчика события DOM с опцией `{ passive: true }`.
- **Использование:**
    
    Прикрепляет к элементу подписчик события. Тип события указывается в параметре. Выражение может быть именем метода, inline-выражением или вовсе отсутствовать, если указан один или несколько модификаторов.
    
    У обычного элемента можно подписаться только [**на нативные события DOM**](https://developer.mozilla.org/en-US/docs/Web/Events). У элемента компонента можно подписаться **на пользовательские события**, вызываемые этим дочерним компонентом.
    
    При работе с нативными событиями DOM, метод получает нативное событие единственным аргументом. В inline-выражениях, можно получить к нему доступ с помощью `$event`: `v-on:click="handle('ok', $event)"`.
    
    Начиная с версии 2.4.0+, `v-on` также поддерживает привязку к объекту пар событие/обработчик без аргумента. Обратите внимание, что при использовании синтаксиса объекта не поддерживаются никакие модификаторы.
    
- **Пример:**
    
    ```
    <!-- обработчик метода -->
    <button v-on:click="doThis"></button>
    
    <!-- динамическое имя события (2.6.0+) -->
    <button v-on:[event]="doThis"></button>
    
    <!-- inline-выражение -->
    <button v-on:click="doThat('hello', $event)"></button>
    
    <!-- сокращённая запись -->
    <button @click="doThis"></button>
    
    <!-- сокращённая запись динамического имени события (2.6.0+) -->
    <button @[event]="doThis"></button>
    
    <!-- модификатор stop propagation -->
    <button @click.stop="doThis"></button>
    
    <!-- модификатор prevent default -->
    <button @click.prevent="doThis"></button>
    
    <!-- модификатор prevent default без дополнительных действий -->
    <form @submit.prevent></form>
    
    <!-- цепочка из модификаторов -->
    <button @click.stop.prevent="doThis"></button>
    
    <!-- модификатор клавиши keyAlias -->
    <input @keyup.enter="onEnter">
    
    <!-- модификатор клавиши keyCode -->
    <input @keyup.13="onEnter">
    
    <!-- обработчик метода будет вызван не больше одного раза -->
    <button v-on:click.once="doThis"></button>
    
    <!-- синтаксис объекта (2.4.0+) -->
    <button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
    ```
    
    Подписка на пользовательское событие в дочернем элементе (обработчик вызывается, когда дочерний элемент вызывает “my-event”):
    
    ```
    <my-component @my-event="handleThis"></my-component>
    
    <!-- inline-выражение -->
    <my-component @my-event="handleThis(123, $event)"></my-component>
    
    <!-- подписываемся на нативное событие в компоненте -->
    <my-component @click.native="onClick"></my-component>
    ```
    

### Как сделать красивый вывод времени[(вверх)](VUE.md)

В компонент добавляется константа

```jsx
const FORMAT_DATE_TIME = new Intl.DateTimeFormat("ru", {
  year: "numeric",
  month: "long",
  day: "numeric",
  hour: "numeric",
  minute: "numeric",
  second: "numeric",
});
```

добавляется в data

```jsx
export default {
  name: "CurrentRagFairBan",
  data() {
    return {
      ...
      formatDateTime: FORMAT_DATE_TIME,
      ...
    }
  },
```

При выводе применяется к типу Date

```jsx
dateTime: this.formatDateTime.format(new Date(Number(profile.dateTime)))
```

## Table[(верх)](VUE.md)

### Как настроить шаблон внутри ячейки таблицы на vue

```jsx
<template>
  <div class="ml-3">
    <b-row class="text-center">
      <b-col md="auto">
        Потенциальная цена - цена, которая была бы установлена. Актуально, только если выключен балансер цен.
      </b-col>
      <b-col>Обнулить статистику</b-col>
      <b-col>
        <b-button-toolbar aria-label="Toolbar with button groups and input groups">
        <b-form-input v-model="searchFilter" placeholder="Search" class="w-25 text-left"></b-form-input>
        <b-button variant="success" @click="searchItems()">Поиск</b-button>
        </b-button-toolbar>
      </b-col>
    </b-row>
    <b-table :fields="fields" :items="items" responsive="sm" bordered>
      <template slot="managerLink" slot-scope="row">
        <table>
          <div class="d-flex flex-column">
          <b-link :href="'/admin/trading/assort/tpl/' + row.item.id + '/price/history'">График цен</b-link>
          <b-link :to="'#'">Обнулить статистику</b-link>
          <b-link :to="'#'">Изменить цену</b-link>
          <b-link :href="'/admin/things/' + row.item.id">Изменить предмет</b-link>
          </div>
        </table>
      </template>
    </b-table>
  </div>
</template>

<script>
export default {
name: "PriceItemsList",
  data() {
    return {
      searchFilter: "",
      fields: [
        { key: 'name', label: 'name' , sortable: true },
        { key: 'fixedPrice', label: 'Fixed price' , sortable: true },
        { key: 'basePrice', label: 'Базовая цена' , sortable: true },
        { key: 'potentialPrice', label: 'Потенциальная цена, руб' , sortable: true },
        { key: 'potentialIncrease', label: 'Потенциальный % роста' , sortable: true },
        { key: 'nowPrice', label: 'Текущая цена, руб' , sortable: true },
        { key: 'increase', label: '% роста' , sortable: true },
        { key: 'minPrice', label: 'Мин. цена продажи' , sortable: true },
        { key: 'maxPrice', label: 'Макс. цена продажи' , sortable: true },
        { key: 'priceSum', label: 'Сумма продаж' , sortable: true },
        { key: 'priceCount', label: 'Кол-во продаж' , sortable: true },
        'managerLink',
      ],
      items: [],
    }
}[(вверх)](VUE%20dae91e89c47648eaad4a575c11514166.md)
```

### Как прописать в цикле темплейты по полям

```jsx
<template
              v-for="field in customTemplateFields"
              #[`cell(${field.key})`]="row"
            >
              <template-field
                :row="row"
                :field="field"
                @copyToClipboard="copyToClipboard"
                @showNicknameHistoryModal="showNicknameHistoryModal"
                @showDetail="showDetail"
                @forwardToProfile="forwardToProfile"
              ></template-field>
</template>
```

### Как настроить шаблон внутри ячейки таблицы на vue

managerLink - несуществующий столбик(в айтемах его нет) в который выводится содержимое темплейта

## Геттер с передачей аргумента[(верх)](VUE.md)

в сторе в геттере

```jsx
	
const getters = {
    getItemNameById: state => id => {
        return state.list[id]
    },
}
```

В методе компонента

```jsx
methods: {
          getItemNameById(id){
            return this.$store.getters["items/getItemNameById"](id);
          },
}
```

## Что дописать в нотификаторе для отображения ошибки[(верх)](VUE.md)

```jsx
<респонс>.text.response.data.error.title
```

## Watch new value[(вверх)](VUE.md)

![Untitled](Работа/VUE/Untitled%203.png)

## $set

вносит поле в реактивную переменную, которое тоже станет реактивным

```jsx
this.$set(object, propertyName, value)
#пример
this.$set(this.book, 'author', 'Достоевский') 
```

## Как ввести функцию как плагин на VUE[(вверх)](VUE.md)

- Подробности
    
    Настройка в main.js
    
    ![Untitled](Работа/VUE/Untitled%204.png)
    
    Настройка самого плагина
    
    ![Untitled](Работа/VUE/Untitled%205.png)
    
    Использование в коде
    
    ![Untitled](Работа/VUE/Untitled%206.png)
    

## Сеттеры Computed свойств[(вверх)](VUE.md)

- Подробности
    
    ![Untitled](Работа/VUE/Untitled%207.png)
    

[(вверх)](VUE.md)

### forEach[(вверх)](VUE.md)

- details
    
    ```php
    val.forEach((item, index) => {
    					let paramName ='searchAid' + index;
    					this.setUrlParameter(paramName, item.aid)
    				});
    ```
    

## Moment[(вверх)](VUE.md)

[Moment.js | Docs](https://momentjs.com/docs/#/displaying/)

## Slots[(вверх)](VUE.md)

Внутри компонента

```jsx
<template>
	<div>
		<slot :name="first" v-bind:item="key"></slot>
		<slot :name="second" v-bind:item="key"></slot>
	</div>
<template>

<script>
export default {
name: "example,
data() {
		key: "user",
	}
}
</script>
```

Снаружи компонента

```sql
<example>
      <template #first="{item}">
        <div>first {{ item }}</div>
      </template>
      <template #second="{item}">
        <div>second {{ item }}</div>
      </template>
</example>
```

будет выведено

```sql
first user
second user
```

## Как чтобы при скролле не уезжал хедер[(вверх)](VUE.md)

Чтобы элемент не уезжал при скролле стиль sticky

```jsx
<template>
	<div>
		<b-button class="mt-2 mb-2 nicknames" variant="primary" size="sm" @click="downloadNicknames">Загрузить ники</b-button>
	</div>
</template>

<style scoped>
.nicknames {
  position: sticky;
	top: 110px;
  left: 280px;
}
</style>
```

## Ошибка в кеше npn при вызове npm install

Если вываливает ошибка

![Untitled](Работа/VUE/Untitled%208.png)

вить

```jsx
sudo npm cache clean -f
```

и повторить 

```jsx
npm i
```

# JS

## валидация существования поля объекта через ?

в примере если `requirements` не существует, выпадет не ошибка а `null`

```sql
area.requirements?.length
```

## Слушать событие([вверх](VUE.md))

Добавляем слушатель

```sql
created() {
	document.addEventListener('keydown', this.changeRangeState)
}
```

добавляем обработку ивента

```sql
methods: {
	changeRangeState(e) {
        if(e.key === "Shift" && !e.repeat) {
          this.myFunctionByShift()
        }
    },
}
```

условие `!e.repeat`  позволяет блочить дополнительные нажатия или если зажали клавишу

условие `e.key === "Shift"` отрабатывает при нажатии клавиши Shift

# Composion Api

## Пример страницы на composion api

- Код целиком
    
    ```sql
    <script setup>
    # импорт методов хуков
    import {
      ref,
      defineProps,
      defineEmits,
      watch,
      computed,
    } from "vue"
    import _ from "lodash"
    import { useStore } from "vuex"
    import moment from "moment"
    import { ElMessageBox } from "element-plus"
    import SeasonEditor from "@/EFTAdmin/Ratings/Components/SeasonEditor.vue";
    
    const HIDE_STATE = "скрыть"
    const SHOW_STATE = "отобразить"
    const ADD_TITLE = "Добавить сезон"
    # константа хранилища сторы
    const store = useStore()
    
    # раздел data
    const editedSeason = ref({})
    const isVisibleEditModal = ref(false)
    const fields = ref([
      { key: "name", label: "Name", sortable: true },
      { key: "dateStart", label: "Date Start", sortable: true },
      { key: "dateEnd", label: "Date End", sortable: true },
      { key: "localizationRu", label: "Localization RU", sortable: true },
      { key: "localizationEn", label: "Localization EN", sortable: true },
      { key: "levelRequired", label: "Level Required", sortable: true },
      { key: "deathsRequired", label: "Deaths Required", sortable: true },
      { key: "hidden", label: "Hidden"},
      { key: "control", label: "" },
    ])
    const timestampFields = ref(["dateStart", "dateEnd"])
    const localizationFields = ref(["localizationRu", "localizationEn"])
    
    # раздел props
    const props = defineProps({
      seasons: {
        type: Object,
        required: true,
      },
      title: {
        type: String,
        required: true
      },
      type: {
        type: String,
        required: true
      },
    })
    
    #раздел emits
    const emit = defineEmits(["updateSeason", "deleteSeason"])
    
    #раздел computed свойств
    const localizations = computed(() => store.getters["ratings/getLocalizations"])
    
    #раздел watch свойств
    watch(props.seasons, () => {
      refreshSeasons()
    })
    
    #раздел methods
    function refreshSeasons() {
      seasons.value = _.deepClone(props.seasons)
    }
    function openEditModal(row, title) {
      editedSeason.value = _.cloneDeep(row)
      editedSeason.value.title = title
      isVisibleEditModal.value = true
    }
    function editSeason(season) {
      editedSeason.value = {...editedSeason.value, ...season}
      emit("updateSeason", editedSeason.value)
      closeEditModal()
    }
    function closeEditModal() {
      isVisibleEditModal.value = false
    }
    async function confirm(title, text) {
      return await ElMessageBox.confirm(
        text,
        title,
        {
          confirmButtonText: 'OK',
          cancelButtonText: 'Cancel',
          type: 'warning',
        }
      )
        .then(() => true)
        .catch(() => false)
    }
    async function deleteSeason(row) {
      const isContinue = await confirm(
        "Пожалуйста, подтвердите",
        `Вы точно хотите удалить сезон ${row.name} ?`
      )
      if (isContinue) {
        emit("deleteSeason", row.id)
      }
    }
    async function changeHideState(row) {
      const text = !row.hidden ? HIDE_STATE : SHOW_STATE
      await confirm(
        "Пожалуйста, подтвердите",
        `Вы точно хотите ${text} сезон ${row.name} ?`
      )
      editedSeason.value = _.cloneDeep(row)
      editedSeason.value.hidden = !editedSeason.value.hidden
      await editSeason()
    }
    
    </script>
    
    <style scoped>
    .icon-size {
      width: 1.5em;
      height: 1.5em;
    }
    .btn-icon-size {
      width: 1em;
      height: 1em;
    }
    </style>
    
    ```
    
- store
    
    ```sql
    import { useStore } from "vuex"
    
    const store = useStore()
    ```
    
- data
    
    ```sql
    const editedSeason = ref({})
    const isVisibleEditModal = ref(false)
    const fields = ref([
      { key: "name", label: "Name", sortable: true },
      { key: "dateStart", label: "Date Start", sortable: true },
      { key: "dateEnd", label: "Date End", sortable: true },
      { key: "localizationRu", label: "Localization RU", sortable: true },
      { key: "localizationEn", label: "Localization EN", sortable: true },
      { key: "levelRequired", label: "Level Required", sortable: true },
      { key: "deathsRequired", label: "Deaths Required", sortable: true },
      { key: "hidden", label: "Hidden"},
      { key: "control", label: "" },
    ])
    const timestampFields = ref(["dateStart", "dateEnd"])
    const localizationFields = ref(["localizationRu", "localizationEn"])
    ```
    
- props
    
    ```sql
    const props = defineProps({
      seasons: {
        type: Object,
        required: true,
      },
      title: {
        type: String,
        required: true
      },
      type: {
        type: String,
        required: true
      },
    })
    ```
    
- emits
    
    ```sql
    const emit = defineEmits(["updateSeason", "deleteSeason"])
    ```
    
- computed
    
    ```sql
    const localizations = computed(() => store.getters["ratings/getLocalizations"])
    ```
    
- watch
    
    ```sql
    watch(props.seasons, () => {
      refreshSeasons()
    })
    ```
    
- methods
    
    ```sql
    function refreshSeasons() {
      seasons.value = _.deepClone(props.seasons)
    }
    function openEditModal(row, title) {
      editedSeason.value = _.cloneDeep(row)
      editedSeason.value.title = title
      isVisibleEditModal.value = true
    }
    function editSeason(season) {
      editedSeason.value = {...editedSeason.value, ...season}
      emit("updateSeason", editedSeason.value)
      closeEditModal()
    }
    function closeEditModal() {
      isVisibleEditModal.value = false
    }
    async function confirm(title, text) {
      return await ElMessageBox.confirm(
        text,
        title,
        {
          confirmButtonText: 'OK',
          cancelButtonText: 'Cancel',
          type: 'warning',
        }
      )
        .then(() => true)
        .catch(() => false)
    }
    async function deleteSeason(row) {
      const isContinue = await confirm(
        "Пожалуйста, подтвердите",
        `Вы точно хотите удалить сезон ${row.name} ?`
      )
      if (isContinue) {
        emit("deleteSeason", row.id)
      }
    }
    async function changeHideState(row) {
      const text = !row.hidden ? HIDE_STATE : SHOW_STATE
      await confirm(
        "Пожалуйста, подтвердите",
        `Вы точно хотите ${text} сезон ${row.name} ?`
      )
      editedSeason.value = _.cloneDeep(row)
      editedSeason.value.hidden = !editedSeason.value.hidden
      await editSeason()
    }
    ```
    
- mount
    
    ```sql
    onMounted(async () => {
      await loadSeasons()
    })
    ```
    
- inject(проброс свойств из родителя во всех детей на любой глубине)
    
    В родительском компоненте
    
    ```sql
    provide("categories", api.value.categories)
    ```
    
    в потомке
    
    ```sql
    const categories = inject("categories")
    ```