---
title: Wprowadzenie
type: guide
order: 2
---

## Czym jest Vue.js?

Vue (wymowa /vjuː/, jak **view**) to **progresywny framework** do budowania interfejsu użytkownika. W przeciwieństwie do monolitycznych frameworków, Vue został zbudowany z myślą o stopnowym implementowaniu. Rdzeń biblioteki koncentruje się tylko na warstwie widoku, łatwo jest z niego skorzystać i zintegrować z innymi bibliotekami w projekcie. Vue w połączeniu z [nowoczesnymi narzędziami](single-file-components.html) i [dodatkowymi bibliotekami](https://github.com/vuejs/awesome-vue#components--libraries).  idealnie sprawdza się jako silnik zaawansowanych jednostronnicowych aplikacji webowych.

Jeżeli jesteś doświadczonym fornt-end developerem i chcesz zobaczyć jak Vue wypada w porównaniu z innymi frameworkami/bibliotekami, przeczytaj [Porówanie Vue z innymi frameworkami](comparison.html).

## Pierwsze kroki

<p class="tip">Oficjalny przewodnik wymaga znajmości HTML, CSS i JavaScript. Rozpoczynanie nauki od Vue nie jest najlepszym rozwiązaniem, lepiej najpierw poznać podstawy i wrócić! Doświadczenie z innymi frameworkami jest pomocne, ale nie wymagane.</p>

Najłatwiejszym sposobem wypróbowania Vue.js jest skorzystanie z [JSFiddle Hello World example](https://jsfiddle.net/chrisvfritz/50wL7mdz/). Otwórz link w nowej karcie i pracuj z przykładami z tego przewodnika. Możesz również pobrać <a href="https://gist.githubusercontent.com/chrisvfritz/7f8d7d63000b48493c336e48b3db3e52/raw/ed60c4e5d5c6fec48b0921edaed0cb60be30e87c/index.html" target="_blank" download="index.html">, następnie utworzyć plik <code>index.html</code> </a> i załączyć do niego Vue:

``` html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

[Instalacja](installation.html) opisuje inne sposoby pracy z Vue. Uwaga: **Nie** zalecamy aby początkujący korzystali z `vue-cli`, zwłaszcza bez znajomości narzędzi bazujących na Node.js.

## Renderowanie deklaratywne

W rdzeniu Vue.js jest system renderujący deklaratywnie DOM, używający składni templatek:

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

Teraz wpisz w konsoli `app3.seen = false`, zobaczysz jak wiadomość znika. Ten przykład pokazuje jak bindować obiekt data nie tylko do tekstu i atrybutów, ale równiez do **struktury** DOM. Ponadto Vue zawiera rozbudowany zbiór efektów przejścia [transition effects](transitions.html), które moga byc automatycznie uruchamiane w momencie wstawiania/usuwania/modyfikowania elementów przez Vue.

Jest jeszcze kilka innych dyrektyw, każda z nich ma swoje funkcjonalności. Np. dyrektywa `v-for` może być wykorzystana do wyświetlenia listy elementów, korzystając z danych w tablicy:

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
      { text: 'Naucz się JavaScript' },
      { text: 'Naucz się Vue' },
      { text: 'Zbuduj coś fajnego' }
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
    { text: 'Naucz się JavaScript' },
    { text: 'Naucz się Vue' },
    { text: 'Zbuduj coś fajnego' }
    ]
  }
})
</script>
{% endraw %}

Wpisz w konsoli `app4.todos.push({ text: 'Nowy element' })`. Powinien się pojawić kolejny element na liście.

## Obsługa wprowadzania danych przez użytkownika

Aby dać użytkownikowi możliwość interakcji z aplikacją, możemy użyć dyrektywy `v-on`, która pozwala na dodanie event listeners do wywoływania metod w instacji Vue:

``` html
<div id="app-5">
  <p>{{ wiadomosc }}</p>
  <button v-on:click="reverseMessage">Odwróc wiadomość</button>
</div>
```
``` js
var app5 = new Vue({
  el: '#app-5',
  data: {
    wiadomosc: 'Hello Vue.js!'
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
  <p>{{ wiadomosc }}</p>
  <button v-on:click="reverseMessage">Odwróc wiadomość</button>
</div>
<script>
var app5 = new Vue({
  el: '#app-5',
  data: {
    wiadomosc: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Zauważ, że w tej metodzie aktualizujemy stan aplikacji bez manipulacji DOM, cała praca jest wykonywana przez Vue, a napisany kod skupia się na podstawowej logice działania.

Vue oferuje również dyrektywę `v-model`, która pozwala na dwukierunkowe bindowanie pomiedzy inputem, a stanem aplikacji.

``` html
<div id="app-6">
  <p>{{ wiadomosc }}</p>
  <input v-model="message">
</div>
```
``` js
var app6 = new Vue({
  el: '#app-6',
  data: {
    wiadomosc: 'Hello Vue!'
  }
})
```
{% raw %}
<div id="app-6" class="demo">
  <p>{{ wiadomosc }}</p>
  <input v-model="message">
</div>
<script>
var app6 = new Vue({
  el: '#app-6',
  data: {
    wiadomosc: 'Hello Vue!'
  }
})
</script>
{% endraw %}

## Korzystanie z komponentów

Kolejnym ważnym konceptem wykorzystywaneym przez Vue, są komponenty. Pozwalają na budowanie dużych aplikacji składających się z małych, samodzielnych i często nadających do ponownego wykorzystania elementów. Niemal każdy typ elementu w interfejsie użytkownika może zostać zorganizowane w drzewie komponentów:

![Component Tree](/images/components.png)

W Vue komponent jest zasadniczo instancją Vue z predefiniowanymi opcjami.
Tak wygląda definicja komponentu todo-item w Vue:

``` js
// Definicja nowego komponentu o nazwie todo-item
Vue.component('todo-item', {
  template: '<li>To jest Todo</li>'
})
```

Teraz możesz skomponować go w szablonie innego komponentu:

``` html
<ol>
  <!-- Tworzy instancję komponentu todo-item -->
  <todo-item></todo-item>
</ol>
```

Ale to jedynie wyświetli ten sam tekst dla każdego elementu do zrobienia, co jest mało uzyteczne. Powinniśmy móc przekazywać dane z zakresu nadrzędnego do komponentów potomnych. Zmodyfikujmy definicję komponentu, aby zaakceptował  [właściwości](components.html#Props):

``` js
Vue.component('todo-item', {
  // Komponent todo-item teraz przyjmuje
  // właściwości jako personalizowane atrybuty.
  // Ta właściwość nazywa się todo
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

Teraz już możemy wstawić do każdego komponentu korzystając z dyrektywy `v-bind`:

``` html
<div id="app-7">
  <ol>
    <!--
      Teraz każdy element todo-item jest reprezentowany przez obiekt todo,
      dzięki czemu jest dynamiczny. Aby to działało potrzbny jest również "key",
      zostanie to wyjasnione w dalszej części przewodnika.
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
    listaZakupow: [
      { id: 0, text: 'Warzywa' },
      { id: 1, text: 'Ser' },
      { id: 2, text: 'Jakiekolwiek inne, byle jadalne' }
    ]
  }
})
```
{% raw %}
<div id="app-7" class="demo">
  <ol>
    <todo-item v-for="item in listaZakupow" v-bind:todo="item" :key="item.id"></todo-item>
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
    listaZakupow: [
      { id: 0, text: 'Warzywa' },
      { id: 1, text: 'Ser' },
      { id: 2, text: 'Jakiekolwiek inne, byle jadalne' }
    ]
  }
})
</script>
{% endraw %}

Poniższy przykład pokazuje jak mozna żadządzać aplikacją przez dzielenie jej na mniejsze elementy, gdzie każdy z potomych jest oddzielony od rodzica interfejscem właściwości. Możemy teraz rozbudować nasz komponent `<todo-item>` o bardziej rozbudowaną teplatkę, oddziaływująca na nadrzędną aplikację.

Zarządzanie dużymi aplikacjacjami wymusza dzielenie kodu na komponenty. W dalszej części przewodnika będzie więcej o [komponentach](components.html), a poniżej jeszcze jeden przykład jak może wyglądać szablon z komponentami:

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

### Praca z własnymi typami elementów DOM

Już dotychczas można było zauważyć, że komponenty Vue bardzo dobrze współpracują z **własnymi typami elementów DOM**, zgodnymi ze specyfikacją [Web Components Spec](https://www.w3.org/wiki/WebComponents/). To dlatego, że składnia komponentów Vue jest luźno wzorowana na tej specyfikacji. Na przykład komponenty Vue implementują [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) wraz z atrybutami. Jednak są pewne różnice:

1. Specyfikacja Web Components jest ciągle w fazie projektowania więc nie jest natywnie wspierana przez wszystkie przeglądarki. Dla porównania komponenty Vue nie potrzebują korzystać z polyfills, a pracują konsekwentnie we wszystkich wspieranych przeglądarkach (IE9 i nowsze).

2. Komponenty Vue zapewniają ważne funkcje, które nie są dostępne w zwykłych własnych typach elementów DOM, takie jak przepływ danych między komponentami, własna komunikacja zdarzeń czy integracja narzędzi.

## Więcej?

Dotychczas wprowadziliśmy Cię w podstawowe funkcjonalności rdzenia Vue.js, pozostała część przewodnika skupi się na bardziej zaawansowanych, zrobi to również bardziej szczegółowo, więc koniecznie przeczytaj do końca!
