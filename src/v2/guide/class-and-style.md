---
title: Bindowanie Klas i Styli
type: guide
order: 6
---

Powszechnym zastosowaniem bindowania wartości jest manipulacja listy klas elementu i jego styli lokalnych (osadzonych w tagu HTML). Obydwie te rzeczy są atrybutami, więc możemy użyć `v-bind` by nimi zarządzać. Do tego, musimy utworzyć stringa będącego wartością tych atrybutów. Niestety, konkatenacja stringów bywa uciążliwa i łatwo jest przy niej popełnić błąd. Z tego powodu Vue oferuje ulepszenie związane z używaniem `v-bind` wraz z atrybutami `class` i `style`. Oprócz stringów, wartością mogą być także obiekty i tablice.

## Bindowanie klas w HTML

### Składnia z użyciem obiektu

Do `v-bind:class` możemy przekazać obiekt żeby dynamicznie przełączać klasy:

``` html
<div v-bind:class="{ active: isActive }"></div>
```

Powyższa składnia determinuje obecność klasy `active` poprzez [prawdziwość](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) własności `isActive`.

Możesz zarządzać większą ilością klas poprzez dodanie wielu atrybutów w obiekcie. Dodatkowo `v-bind:class` może współistnieć ze standardowym atrybutem `class`, tak jak w przykładzie poniżej.

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

Podczas zmiany `isActive`, lub `hasError` lista klas będzie odpowienio aktualizowana. Jeżeli na przykład właściwość atrybutu `hasError` zostanie zmieniona na `true` lista klas zmieni się na `"static active text-danger"`.

Zbindowany obiekt nie musi być wstawiany lokalnie:

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

Kod przedstawiony niżej spowoduje renderowanie z takim samym rezultatem. Możemy bindować także [computed property](computed.html), który zwraca obiekt. Jest to powszechny i dobry wzorzec.

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

Abo odnieść się do listy klas, do `v-bind:class` możemy przekazać tablicę.

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

### With Components

> Ta sekcja zakłada, że zapoznałeś się już z [Vue Components](components.html). Jeżeli nie, omiń ją i wróć tutaj póżniej.

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

Kiedy `isActive` będzie prawdziwe, otrzymmy taki HTML:

``` html
<p class="foo bar active">Hi</p>
```

## Bindowanie stylów lokalnych

### Składnia z użyciem obiektu

The object syntax for `v-bind:style` is pretty straightforward - it looks almost like CSS, except it's a JavaScript object. You can use either camelCase or kebab-case (use quotes with kebab-case) for the CSS property names:

``` html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
``` js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

It is often a good idea to bind to a style object directly so that the template is cleaner:

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

Again, the object syntax is often used in conjunction with computed properties that return objects.

### Array Syntax

The array syntax for `v-bind:style` allows you to apply multiple style objects to the same element:

``` html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

### Auto-prefixing

When you use a CSS property that requires [vendor prefixes](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix) in `v-bind:style`, for example `transform`, Vue will automatically detect and add appropriate prefixes to the applied styles.

### Multiple Values

> 2.3.0+

Starting in 2.3.0+ you can provide an array of multiple (prefixed) values to a style property, for example:

``` html
<div v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

This will only render the last value in the array which the browser supports. In this example, it will render `display: flex` for browsers that support the unprefixed version of flexbox.
