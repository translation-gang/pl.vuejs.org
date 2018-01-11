---
title: Instancja Vue
type: guide
order: 3
---

## Tworzenie instancji Vue

Budowa każdej aplikacji Vue, zaczyna się od utworzneia **instacji Vue** za pomocą funkcji `Vue`:

```js
var vm = new Vue({
  // opcje
})
```

Vue nie jest ściśle powiązany z [wzorcem MVVM](https://en.wikipedia.org/wiki/Model_View_ViewModel), ale jest mocno nim inspirowany. Konwencją jest odwoływanie do zmiennej `vm` (skrót dla ViewModel) aby odnieść się do instancji Vue.

Podczas tworzenia instacji Vue, tworzony jest **obiekt opcje**. Ten przewodnik opisuje, jak go konstruować aby uzyskać określone zachowanie. Pełną listę opcji znajdziesz w [dokumentacji API](../api/#Options-Data).

Aplikacja Vue zawiera **główną instancję Vue**, utworzonej poleceniem `new Vue`, opcjonalnie może być zorganizowana w strukturę drzewiastą, zagnieżdzonych komponentów wielokrotnego uzytku. Przykładowe drzewo komponetnów dla aplikacji todo może wyglądać następująco:

```
Główna Instacja
└─ ListaTodo
   ├─ ElementTodo
   │  ├─ ButtonUsunTodo
   │  └─ ButtonEdytujTodo
   └─ StopkaListyTodo
      ├─ ButtonWyczyscTodo
      └─ StatystykaListyTodo
```
Dalej omówimy bardziej szczegółowo [system komponentów](components.html). Teraz istotny jest fakt, że wszystkie komponenty Vue jak i instacje Vue przyjmują takie same właściwości obiektu opcje (poza kilkoma specyficznymi opcjami dla głównej instancji).

## Dane i metody

Instancja Vue po utworzeniu dodaje wszystkie właściwości znalezione w obiekcie `data` do **reaktywnego systemu** Vue. Jeżeli wartość ktrórejkolwiek z właściwości zostanie zmieniona, widok "zareaguje" aktualizacją wartości.

```js
// Obiekt data
var data = { a: 1 }

// Obiekt data wstawiony do instacji Vue
var vm = new Vue({
  data: data
})

// To jest odwołanie do tego samego obiektu
vm.a === data.a // => true

// Zmiana właściwości w instancji
// zmienia również dane w oryginalnej lokalizacji
vm.a = 2
data.a // => 2

// ... i vice-versa
data.a = 3
vm.a // => 3
```

Gdy zmienią się wartości w obiekcie data, widok je zaktualizuje. Trzeba jednak pamiętać, że właściwości w `data` są **reaktywne** tylko jeżeli istnieją w utworzonej instancji. To oznacza, że jeżeli dodasz nową właściwość np:

```js
vm.b = 'cześć'
```

Zmiany w `b` nie wywołają zmian w widoku. Jeżeli przewidujesz, że będziesz potrzebować tej właściwości później ale na początku nie istnieje lub jest pusta, musisz ustawić wartość początkową np:

```js
data: {
  NowyTekstTodo: '',
  licznikWyswietlen: 0,
  uktyjZakonczoneZadania false,
  zadania: [],
  error: null
}
```

Jedynym wyjątkiem jest użycie `Object.freeze()`, zabezpiecza to istniejące wartości przed zmianą co oznacza, że reaktywny system nie może śledzić zmian.

```js
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data () {
    return {
      obj
    }
  }
})
```

```html
<div id="app">
  <p>{{ obj.foo }}</p>
  <!-- to nie zaktualizuje obj.foo! -->
  <button @click="obj.foo = 'baz'">Zmień</button>
</div>
```

Vue udostępnia wiele przydatnych własciwości i metod dla obiekty data. Mają one dla odróżnienia prefix `$`, jak w poniższym przykladzie:

```js
var data = { a: 1 }
var vm = new Vue({
  el: '#przyklad',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('przyklad') // => true

// $watch jest instancją metody
vm.$watch('a', function (nowaWartosc, staraWartosc) {
  // To wywołanie wsteczne zostanie zrealizowane gdy zmieni się `vm.a`.
})
```

Pełną listę właściwości i metdod znajdziesz w [Dokumentacji API](../api/#Instance-Properties)

## Uchwyty cykli życia Instancji

Każda instancja Vue realizuje serię kroków inicjalizujących podczas jej tworzenia, np: ustawnenia powiązania danych, kompiluje szablony, iniccjalizuje instancję w DOM i aktualizuje DOM w przypadku zmiany danych. Po drodze uruchamia funkcje tzw. **uchwyty cyklu życia** dając możliwość użytkownikowi definicji własnych stanów.

Przykładowo uchwyt [`created`](../api/#created) może być wykorzystany do uruchomienia kodu po utworzeniu instancji:

```js
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` wskazuje na instancję Vue
    console.log('a is: ' + this.a)
  }
})
// => "a wynosi: 1"
```

Są również inne uchwyty, które mogą być wywołane w różnych stadiach cyklu zycia instancji, tak jak [`mounted`](../api/#mounted), [`updated`](../api/#updated), i [`destroyed`](../api/#destroyed). Wszytskie uchwyty cykli życia są wywoływane z własnym kontekstem `this` do wywołania go.

<p class="tip">W właściwościach odwołania zwrotnego nie używaj [funkcji strzałkowych](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) tak, jak w `created: () => console.log(this.a)` lub `vm.$watch('a', newValue => this.myMethod())`. Ponieważ funkcje strzałkowe sa powiązane z kontekstem nadrzędnym, `this` nie będzie się odnosił do instancji, której się spodziewasz, często skutkuje to błedami: `Uncaught TypeError: Cannot read property of undefined` lub `Uncaught TypeError: this.myMethod is not a function`.</p>

## Diagram cyklu życia

Poniższy diagram cyklu żcyia nie musi być dla ciebie w tej chwili calkowicie zrozumiały, ale wraz z doswiadczeniem staje się przydatny.

![Cykl życia instancji Vue](/images/lifecycle.png)
