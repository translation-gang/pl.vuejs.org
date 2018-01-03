---
title: Introduction
type: guide
order: 2
---

## Czym jest Vue.js?

Vue (wymowa /vjuː/, jak **view**) to **progresywny framework** do budowania interfejsu użytkownika. W  is a **progressive framework** for building user interfaces. W przeciwieństwie do monolitycznych frameworków, Vue został zbudowany z myślą o stopnowym implementowaniu. Rdzeń biblioteki koncentruje się tylko na warstwie widoku, łatwo jest z niego skorzystać i zintegrować z innymi bibliotekami w projekcie. Vue w połączeniu z [nowoczesnymi narzędziami](single-file-components.html) i [dodatkowymi bibliotekami](https://github.com/vuejs/awesome-vue#components--libraries).  idealnie sprawdza się jako silnik zaawansowanych jednostronnicowych aplikacji webowych.

Jeżeli jesteś doświadczonym fornt-end developerem i chcesz zobaczyć jak Vue wypada w porównaniu z innymi frameworkami/bibliotekami, przeczytaj [Porówanie Vue z innymi frameworkami](comparison.html).

## Pierwsze kroki

<p class="tip">Oficjalny przewodnik wymaga znajmości HTML, CSS i JavaScript. Rozpoczynanie nauki od Vue nie jest najlepszym rozwiązaniem, lepiej najpierw poznać podstawy i wrócić! Doświadczenie z innymi frameworkami jest pomocne, ale nie wymagane.</p>

Najłatwiejszym sposobem wypróbowania Vue.js jest skorzystanie z [JSFiddle Hello World example](https://jsfiddle.net/chrisvfritz/50wL7mdz/). Otwórz link w nowej karcie i pracuj z przykładami z tego przewodnika. Możesz również pobrać <a href="https://gist.githubusercontent.com/chrisvfritz/7f8d7d63000b48493c336e48b3db3e52/raw/ed60c4e5d5c6fec48b0921edaed0cb60be30e87c/index.html" target="_blank" download="index.html">, następnie utworzyć plik <code>index.html</code> </a> i załączyć do niego Vue:

``` html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

[Instalacja](installation.html) opisuje inne sposoby pracy z Vue. Uwaga: **Nie** zalecamy aby początkujący korzystali z `vue-cli`, zwłaszcza jeżeli nie znasz narzędzi bazujących na Node.js.

## Renderowanie deklaratywne

W rdzeniu Vue.js jest system renderujący deklaratywnie DOM, używając składni templatek:

``` html
<div id="app">
  {{ message }}
</div>
```
``` js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```
{% raw %}
<div id="app" class="demo">
  {{ message }}
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
</script>
{% endraw %}

To Twoja pierwsza aplikacja w Vue! Wygląda bardzo prosto, jednak wymagała od Vue sporo pracy, której nie widać. Obiekt 'data' oraz DOM zoststały połączone i wszystko jest już **reaktywne**. Skąd o tym wiemy? Otwórz konsolę (w tym oknie przeglądarki) i ustaw inną wartość w `app.message`. Przykład powinien się zaktualizować.

Oprócz tekstu, możesz również bindować atrybuty:

``` html
<div id="app-2">
  <span v-bind:title="message">
    Najedź na mnie myszką i zaczekaj kilka sekund, aby zobaczyć dynamicznie bindowany title!
  </span>
</div>
```
``` js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Strona wczytana ' + new Date().toLocaleString()
  }
})
```
{% raw %}
<div id="app-2" class="demo">
  <span v-bind:title="message">
    Najedź na mnie myszką i zaczekaj kilka sekund, aby zobaczyć dynamicznie bindowany title!
  </span>
</div>
<script>
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Strona wczytana ' + new Date().toLocaleString()
  }
})
</script>
{% endraw %}

Tutaj pojawia się coś nowego. Atrybut `v-bind` widoczny tutaj, nazywany jest **dyrektywą**. Dyrektywy są poprzedzane `v-` aby zaznaczyć, że są to specjalne atrybuty dla Vue i jak się zapewne domyślasz umożliwiają reaktywne zachowanie w renderowanym DOM. W tym wypadku mówi: "utrzymuj wartość atrybutu `title` tego elementu, taką jak właściwość `message` w instancji Vue".

Jeżeli ponownie otworzysz konsolę przeglądarki i wpiszesz: `app2.message = 'jakaś nowa wiadomość'` raz jeszcze zaktualizujesz DOM, tym razem atrybut `title`.

## Warunki i pętle

Przełączanie widoczności elementu jest również bardzo łatwe:

``` html
<div id="app-3">
  <span v-if="seen">Widać mnie</span>
</div>
```

``` js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

{% raw %}
<div id="app-3" class="demo">
  <span v-if="seen">Widać mnie</span>
</div>
<script>
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
</script>
{% endraw %}

Teraz wpisz w konsoli `app3.seen = false`, zobaczysz jak wiadomość znika.

This example demonstrates that we can bind data to not only text and attributes, but also the **structure** of the DOM. Moreover, Vue also provides a powerful transition effect system that can automatically apply [transition effects](transitions.html) when elements are inserted/updated/removed by Vue.

There are quite a few other directives, each with its own special functionality. For example, the `v-for` directive can be used for displaying a list of items using the data from an Array:

``` html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
``` js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
```
{% raw %}
<div id="app-4" class="demo">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
<script>
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
</script>
{% endraw %}

In the console, enter `app4.todos.push({ text: 'New item' })`. You should see a new item appended to the list.

## Handling User Input

To let users interact with your app, we can use the `v-on` directive to attach event listeners that invoke methods on our Vue instances:

``` html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
```
``` js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
{% raw %}
<div id="app-5" class="demo">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
<script>
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Note that in this method we update the state of our app without touching the DOM - all DOM manipulations are handled by Vue, and the code you write is focused on the underlying logic.

Vue also provides the `v-model` directive that makes two-way binding between form input and app state a breeze:

``` html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
``` js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```
{% raw %}
<div id="app-6" class="demo">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
<script>
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
</script>
{% endraw %}

## Composing with Components

The component system is another important concept in Vue, because it's an abstraction that allows us to build large-scale applications composed of small, self-contained, and often reusable components. If we think about it, almost any type of application interface can be abstracted into a tree of components:

![Component Tree](/images/components.png)

In Vue, a component is essentially a Vue instance with pre-defined options. Registering a component in Vue is straightforward:

``` js
// Define a new component called todo-item
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})
```

Now you can compose it in another component's template:

``` html
<ol>
  <!-- Create an instance of the todo-item component -->
  <todo-item></todo-item>
</ol>
```

But this would render the same text for every todo, which is not super interesting. We should be able to pass data from the parent scope into child components. Let's modify the component definition to make it accept a [prop](components.html#Props):

``` js
Vue.component('todo-item', {
  // The todo-item component now accepts a
  // "prop", which is like a custom attribute.
  // This prop is called todo.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

Now we can pass the todo into each repeated component using `v-bind`:

``` html
<div id="app-7">
  <ol>
    <!--
      Now we provide each todo-item with the todo object
      it's representing, so that its content can be dynamic.
      We also need to provide each component with a "key",
      which will be explained later.
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id">
    </todo-item>
  </ol>
</div>
```
``` js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
```
{% raw %}
<div id="app-7" class="demo">
  <ol>
    <todo-item v-for="item in groceryList" v-bind:todo="item" :key="item.id"></todo-item>
  </ol>
</div>
<script>
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
</script>
{% endraw %}

This is a contrived example, but we have managed to separate our app into two smaller units, and the child is reasonably well-decoupled from the parent via the props interface. We can now further improve our `<todo-item>` component with more complex template and logic without affecting the parent app.

In a large application, it is necessary to divide the whole app into components to make development manageable. We will talk a lot more about components [later in the guide](components.html), but here's an (imaginary) example of what an app's template might look like with components:

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

### Relation to Custom Elements

You may have noticed that Vue components are very similar to **Custom Elements**, which are part of the [Web Components Spec](https://www.w3.org/wiki/WebComponents/). That's because Vue's component syntax is loosely modeled after the spec. For example, Vue components implement the [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) and the `is` special attribute. However, there are a few key differences:

1. The Web Components Spec is still in draft status, and is not natively implemented in every browser. In comparison, Vue components don't require any polyfills and work consistently in all supported browsers (IE9 and above). When needed, Vue components can also be wrapped inside a native custom element.

2. Vue components provide important features that are not available in plain custom elements, most notably cross-component data flow, custom event communication and build tool integrations.

## Ready for More?

We've briefly introduced the most basic features of Vue.js core - the rest of this guide will cover them and other advanced features with much finer details, so make sure to read through it all!
