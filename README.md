# Vue.js

## 1. Introduction

Vue est un framework JS évolutif permettant de concevoir des UI.

Il est accessible (connaissances basiques HTML CSS et JS), polyvalent (mi-chemin entre Framework et Librairie) et performant (minimum utile de 20ko, intégration via CDN, virtual DOM).

Similitudes avec les autres framework/librairies, mais différences qui le rendent bien plus agréable à utiliser.

## 2. Principes

### a. Rendu déclaratif

* **Afficher des données**

Similaire à Angular et React.

```html
<div id="app">
  {{ message }}
</div>
```

```js
const app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue !'
  }
})
```

### b. Directives

Similaire à Angular.

* **`v-bind`**

```html
<div id="app-2">
  <span v-bind:title="message">
    Passez votre souris sur moi pendant quelques secondes pour voir mon titre lié dynamiquement !
  </span>
</div>
```

```js
const app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Vous avez affiché cette page le ' + new Date().toLocaleString()
  }
})
```

Abréviation en utilisant uniquement `:`.

Possibilité d'utiliser des expressions JS directement dans le template.

* **`v-for`**

```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```js
const app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Apprendre JavaScript' },
      { text: 'Apprendre Vue' },
      { text: 'Créer quelque chose de génial' }
    ]
  }
})
```

* **`v-if` et `v-show`**

`v-if` n'inclut pas l'élément dans le DOM, `v-show` le masque seulement (équivalent attribut `hidden`).

```html
<div id="app-3">
  <p v-if="seen">Maintenant vous me voyez</p>
</div>
```

```js
const app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

* **`v-on`**

Gestion des évènements, similaire à Angular et React.

```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Message retourné</button>
</div>
```

```js
const app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js !'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```

Abréviation en utilisant `@`.


* **`v-model`**

Binding bidirectionnel entre un input et une variable, similaire à Angular.

```html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```

```js
const app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue !'
  }
})
```

## 3. Composants

Concept majeur de Vue, similaire à Angular, React, Svelte, etc.

### a. Création

```js
Vue.component('todo-item', {
  template: '<li>Ceci est une liste</li>'
})

const app = new Vue(...)
```

```html
<ol>
  <!-- Syntaxe Angular-like -->
  <todo-item></todo-item>

  <!-- Syntaxe JSX-like -->
  <todo-item/>
</ol>
```

### b. Props et Data

Une `prop` est l'équivalent d'un `@Input()` en Angular.

```js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

const app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Légumes' },
      { id: 1, text: 'Fromage' },
      { id: 2, text: 'Tout ce que les humains sont supposés manger' }
    ]
  }
})
```

```html
<div id="app-7">
  <ol>
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    />
  </ol>
</div>
```

### c. Valeurs calculées

Principalement utilisées pour formatter les données, équivalent des `getters` en Angular.

```html
<div>
  {{ message.split('').reverse().join('') }}
</div>
```

```js
new Vue.Component('example', {
    data: {
        message: 'Bonjour'
    },
    computed: {
        reversedMessage() {
            return this.message.split('').reverse().join('')
        }
    }
})
```

```html
<div>
  <p>Message original : {{ message }}</p>
  <p>Message inversé calculé : {{ reversedMessage }}</p>
</div>
```

### d. Méthodes

Equivalent aux méthodes en Angular, principalement utilisées pour gérer les évènements ou faire des appels vers une API.

```html
<div>
  <button v-on:click="increment">Incrémenter</button>
  <p>Le bouton ci-dessus a été cliqué {{ counter }} fois.</p>
</div>
```

```js
Vue.Component('counter', {
    data: {
        counter: 0
    },
    methods: {
        increment() {
            this.counter++   
        }
    }
})
```

## 4. Evolutions

Tout ce qui a été présenté jusqu'ici est amplement suffisant pour développer une application
front de petite envergure mais rendra le développement d'applications complexes plutôt difficile
pour plusieurs causes :

* Les définitions globales forcent à avoir un nom unique pour chaque composant

* Les templates sous forme de chaines de caractères ne bénéficient pas de la coloration syntaxique et requièrent l’usage de slashes disgracieux pour le HTML multiligne.

* L’absence de support pour le CSS signifie que le CSS ne peut pas être modularisé comme HTML et JavaScript

* L’absence d’étape de build nous restreint au HTML et à JavaScript ES5, sans pouvoir utiliser des préprocesseurs tels que Babel ou Pug (anciennement Jade).

De plus, dans le cas d'une application monopage on aura certainement besoin d'un router ou d'un outil de state management.

### a. Composant monofichier

Les composants monofichier résolvent les désavantages précédents puisqu'ils
permettent de modulariser les composants en incluant l'affichage (HTML), la logique (JS) et le style (CSS) à un seul
et même endroit (fichier `.vue`) à l'aide d'outils de build tels que Webpack ou Browserify.

Il est également possible d'y utiliser des préprocesseurs tels que Pug (templating), Babel (ES Modules) ou SCSS.


```vue
<template lang="pug">
    p {{ greeting }} World!
    OtherComponent
</template>

<script>
import OtherComponent from './OtherComponent.vue'

export default Hello {
    components: {
        OtherComponent
    },
    data() {
        return {
            greeting: 'Hello'
        }
    }
}
</script>

<style lang="scss" scoped>
p {
    font-size: 20px;
    text-align: center;
}
</style>
```

L'attribut `scoped` de la balise `<style>` permet d'empêcher l'application de toutes les règles de style définies
en dehors du fichier de définition du composant.

### b. VueRouter

Similaire au Router d'Angular.

Possibilité d'intégration via CDN.

```html
<div id="app">
  <p>
    <router-link to="/foo">Aller à Foo</router-link>
    <router-link to="/bar">Aller à Bar</router-link>
  </p>
  <router-view></router-view>
</div>
```

L'élément `<router-link>` est l'équivalent d'un lien possédant l'attribut `routerLink` en Angular.

L'élément `<router-view>` est l'équivalent du `<router-outlet>` en Angular.

```js
import Foo from './components/Foo.vue'
const Bar = () => import('./components/Bar.vue') // Lazy loading

const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]

export default AppRouter = new VueRouter({
  routes
})
```

Il ne reste plus qu'à inclure l'objet `AppRouter` dans l'instance de Vue de l'application.
### c. Vuex

### d. Server-side rendering

## 5. Vue vs Angular

## 6. Vue 3

