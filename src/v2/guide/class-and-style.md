---
title: Bindowanie Klas i Styli
type: guide
order: 6
---

Powszechnym zastosowaniem bindowania wartości jest manipulacja listy klas elementu i jego styli lokalnych (osadzonych w tagu HTML). Obydwie te rzeczy są atrybutami, więc możemy użyć `v-bind` by nimi zarządzać. Do tego, musimy utworzyć string będącego wartością tych atrybutów. Niestety, konkatenacja stringów bywa uciążliwa i łatwo jest przy niej popełnić błąd. Z tego powodu Vue oferuje ulepszenie związane z używaniem `v-bind` wraz z atrybutami `class` i `style`. Oprócz stringów, wyrażenia te mogą oddziaływać również na obiekty i tablice.

## Bindowanie klas w HTML

### Składnia z użyciem obiektu

Do `v-bind:class` możemy przekazać obiekt żeby dynamicznie przełączać klasy:

``` html
<div v-bind:class="{ active: isActive }"></div>
```

Powyższa składnia determinuje obecność klasy `active` poprzez [prawdziwość](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) własności `isActive`.

Możesz zarządzać większą ilością klas poprzez dodanie wielu własności w obiekcie. Dodatkowo `v-bind:class` może współistnieć ze standardowym atrybutem `class`, tak jak w przykładzie poniżej.

``` html
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

Z następującymi danymi:

``` js
data: {
  isActive: true,
  hasError: false
}
```

Wyrenderuje:

``` html
<div class="static active"></div>
```

Podczas zmiany `isActive`, lub `hasError` lista klas będzie odpowienio aktualizowana. Jeżeli na przykład wartość własności `hasError` zostanie zmieniona na `true` lista klas zmieni się na `"static active text-danger"`.

Powiązany obiekt nie musi być wstawiany lokalnie:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

Kod przedstawiony niżej spowoduje renderowanie z takim samym rezultatem. Możemy bindować także [Wartości wyliczone](computed.html), który zwraca obiekt. Jest to powszechny i dobry wzorzec.

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

### Składnia z użyciem tablicy

Aby odnieść się do listy klas, do `v-bind:class` możemy przekazać tablicę.

``` html
<div v-bind:class="[activeClass, errorClass]"></div>
```
``` js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

Spowoduje to wyrenderowanie:

``` html
<div class="active text-danger"></div>
```

Jeżeli chcesz przełączać klasy w zależności od jakiegoś warunku możesz to zrobić korzystając z potrójnego wyrażenia warunkowego.

``` html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

Dzięku temu zapisowi klasa `errorClass` będzie aktywna zawsze, za to klasa `activeClass` będzie aktywna tylko gdy wartość `isActive` będzie prawdziwa.

Jednakże powstałe w ten sposób wyrażenia mogą być rozwlekłe. Właśnie dlatego możemy użyć składni z użyciem obiektu wewnątrz składni tablicowej.

``` html
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

### W komponencie

> Ta sekcja zakłada, że zapoznałeś się już z [Komponentami Vue](components.html). Jeżeli nie, zrób to i wróć tutaj póżniej.

Kiedy używasz atrybutu `class` na utworzonym przez ciebie komponencie, klasy zostaną do niego dodane. Nie nadpiszą one klas wewnątrz tego komponentu.

Na przykład jeżeli zadeklarujesz taki komponent:

``` js
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```

Następnie dodasz do niego klasy w ten sposób:

``` html
<my-component class="baz boo"></my-component>
```

Wyrenderowany HTML będzie wyglądał następująco:

``` html
<p class="foo bar baz boo">Hi</p>
```

To samo odnośi się do bindowania klas:

``` html
<my-component v-bind:class="{ active: isActive }"></my-component>
```

Kiedy `isActive` będzie prawdziwe, otrzymamy taki HTML:

``` html
<p class="foo bar active">Hi</p>
```

## Wiązanie stylów lokalnych

### Składnia z użyciem obiektu

Składnia z użyciem obiektu w `v-bind:style` jest dość prosta, wygląda podobnie jak przy użyciu zwykłego CSSa, z tym, że musimy użyć obiektu. Do określania własności CSS możesz użyć camelCase, lub kebab-case (przy używaniu kebab-case musisz używać cudzysowów):

``` html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
``` js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

Dobrą praktyką jest wiązanie atrybutu `style` tylko z użyciem obiektu, by kod był bardziej przejrzysty.

``` html
<div v-bind:style="styleObject"></div>
```
``` js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

Składnia obiektowa jest często używana w połączeniu z wartościami wyliczonymi, które zwracają obiekt.

### Składnia z użyciem tablicy

Składnia z użyciem tablicy w `v-bind:style` pozwala ci na dodanie wielu obiektów z właściwościami CSS do tego samego elementu:

``` html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

### Automatyczne dodawanie prefixów

Kiedy w `v-bind:style` używamy własności CSS która wymaga [prefixów](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix), na przykład `transform`, Vue automatycznie to wykryje i doda za nas wymagane prefixy.


### Wiele wartości

> 2.3.0+

Od wersji 2.3.0 w zwyż możemy używać wielu wartości (prefixów) dla danej właściwości w obiekcie, na przykład:

``` html
<div v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

Powyższe wyrenderuje tylko jedną wartość, tą która jest wymgana przez przeglądarkę. Dla przykładu `disply: flex` zostanie wyrenderowane tylko w przeglądarkach, które nie wymagają żadnych prefixów do użycia `disply: flex`.
