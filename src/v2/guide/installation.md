---
title: Instalacja
type: guide
order: 1
vue_version: 2.5.13
gz_size: "30.67"
---

### Uwaga dotycząca zgodności

Vue **nie** nie działa na IE8 i niższych wersjach, ponieważ wykorzystuje cechy ECMAScript 5 których nie wspiera IE8. Wszystkie pozostałe przeglądarki są obsługiwane szczegóły: [ECMAScript 5 compliant browsers](http://caniuse.com/#feat=es5).

### Informacje o wydaniu

Najnowsza stabilna wersja: {{vue_version}}

Szczegółowe informacje o każdej wersji są dostępne na stronie [GitHub](https://github.com/vuejs/vue/releases).

## Vue Devtools

Podczas pracy z Vue, zalecamy zainstalowanie w przeglądarce [Vue Devtools](https://github.com/vuejs/vue-devtools#vue-devtools) dodatek ten pozwala na podgląd i debugowanie aplikacji w przyjaznym interfejsie użytkownika.

## Bezpośredie załączanie w `<script>`

Poprostu pobierz i załącz w tagu script. 'Vue' zostanie zarejestrowane jako zmienna globalna.

<p class="tip">W trakcie prac nad aplikacją nie używaj wersji produkcyjnej, ponieważ nie zawiera obsługi najpopularniejszych błędów przez ostrzeżenia!</p>

<div id="downloads">
<a class="button" href="/js/vue.js" download>Wersja developerska</a><span class="light info">zawiera wszystkie ostrzeżenia i tryb debug</span>

<a class="button" href="/js/vue.min.js" download>Wersja produkcyjna</a><span class="light info">Pozbawiona ostrzeżeń i zminifikowana, {{gz_size}}KB min+gzip</span>
</div>

### CDN

Zalecamy załączanie z linku: [https://cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue), link zawiera najnowszą wersję, zaraz po publikacji na npm. Możesz również zajrzeć do źródła paczki npm na: [https://cdn.jsdelivr.net/npm/vue/](https://cdn.jsdelivr.net/npm/vue/).

Dostępne również [unpkg](https://unpkg.com/vue) oraz [cdnjs](https://cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.js) (cdnjs potrzebuje czasu na synchronizację, dlatego najnowsza wersja może nie być tu dostępna od razu).

## NPM

Podczas budowy dużych aplikacji z Vue zalecamy korzystanie z NPM. Dobrze współpracuje z [Webpack](https://webpack.js.org/) i [Browserify](http://browserify.org/). Vue zapewnia również dodatkowe narzędzia do authoringu [Single File Components](single-file-components.html).

``` bash
# Najnowsza stabilna wersja
$ npm install vue
```

## CLI

Vue.js zapewnia [oficjalne CLI](https://github.com/vuejs/vue-cli) do szybkiej budowy szkieletu jednostronnicowych aplikacji. Dzięki temu dostajesz zestaw narzędzi do nowoczesnej pracy z frontendem. Wystarczy kilka minut aby uruchomić takie funkcje jak hot-reload, lint-on-save czy production-ready:

``` bash
# zainstaluj vue-cli
$ npm install --global vue-cli
# utwórz nowy projekt korzystając z szablonu "webpack"
$ vue init webpack my-project
# zainstaluj zależne elementy i działaj!
$ cd my-project
$ npm install
$ npm run dev
```

<p class="tip">CLI wymaga znajomości Node.js i powiązanych z nim narzędzi. Jeżeli dopiero zaczynasz z Vue lub narzedziami front-endowymi, zalecamy zapoznanie się z <a href="./">przewodnikiem</a> nie korzystając z narzędzi przed rozpoczęciem korzystania z CLI.</p>

## Wyjaśnienie różnych buildów

W katalogu [`dist/` z paczki NPM](https://cdn.jsdelivr.net/npm/vue/dist/) znajdziesz wiele różnych buildów Vue.js. Poniżej krótkie zestawienie róźnic pomiędzy nimi:

| | UMD | CommonJS | ES Module |
| --- | --- | --- | --- |
| **Full** | vue.js | vue.common.js | vue.esm.js |
| **Runtime-only** | vue.runtime.js | vue.runtime.common.js | vue.runtime.esm.js |
| **Full (production)** | vue.min.js | - | - |
| **Runtime-only (production)** | vue.runtime.min.js | - | - |

### Terminologia

- **Full**: buildy zawierające środowisko wykonawcze i kompilator.

- **Compiler**: kod odpowiadający za kompilowanie templatek do funkcji JavaScript.

- **Runtime**: kod odpowiadający za tworzenie instancji Vue, renderowanie i zarządzanie wirtualnym DOMem itp. Krótko mówiąc za wszystko oprócz komilatora.

- **[UMD](https://github.com/umdjs/umd)**: build UMD może być użyty bezpośrednio w przegladarce w tagu `<script>`. Domyślny plik jsDelivr CDN pod adresem [https://cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue) zawiera środowisko wykonawcze + kompilator UMD build (`vue.js`).

- **[CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1)**: CommonJS builds are intended for use with older bundlers like [browserify](http://browserify.org/) or [webpack 1](https://webpack.github.io). The default file for these bundlers (`pkg.main`) is the Runtime only CommonJS build (`vue.runtime.common.js`).

- **[ES Module](http://exploringjs.com/es6/ch_modules.html)**: są przeznaczone do użytku z nowoczesnymi programami typu bundlers, takimi jak
[webpack 2](https://webpack.js.org) lub [rollup](https://rollupjs.org/). Domyślny plik dla tych bundlerów (`pkg.module`) zawiera tylko środowisko wykonawcze ES Module build (`vue.runtime.esm.js`).

### Runtime + Compiler vs. Runtime-only

Jeżeli potrzebujesz renderować szablony po stronie klienta (np. przekazać string do `template`, lub utworzyć element wykorzystując in-DOM HTML jako template), będziesz potrzebował kompilatora, a więc pełnego builda:

``` js
// ten kod potrzebuje kompilatora
new Vue({
  template: '<div>{{ hi }}</div>'
})

// ten nie potrzebuje kompilatora
new Vue({
  render (h) {
    return h('div', this.hi)
  }
})
```

Jeżeli używasz `vue-loader` lub `vueify`, szablony wewnątrz plików `*.vue` są prekompilowane do JavaScript w locie, nie porzebujesz kompilatora w finalnej wersji pliku wystarczy runtime-only build.

Ponieważ pliki runtime-only są około 30% lżejsze od swoich wersji full-build, powinno się ich używać gdzie tylko to możliwe. Jednak w razie potrzeby zawsze można wykonać full-build konfigurując alias w bundlerze:

#### Webpack

``` js
module.exports = {
  // ...
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js' // 'vue/dist/vue.common.js' dla webpack 1
    }
  }
}
```

#### Rollup

``` js
const alias = require('rollup-plugin-alias')

rollup({
  // ...
  plugins: [
    alias({
      'vue': 'vue/dist/vue.esm.js'
    })
  ]
})
```

#### Browserify

Dodaj do projektu `package.json`:

``` js
{
  // ...
  "browser": {
    "vue": "vue/dist/vue.common.js"
  }
}
```

### Development vs. Production Mode

Tryb deweloperski/produkcyjny jest hard-kodowany w buildach UMD: nie minifikowane pliki są na potrzeby deweloperskie, a zminifikowane na potrzeby producji.

Buildy CommonJS i ES Module są przeznaczone dla bundlerów, dlatego nie załączyliśmy w nich wersji minifikowanej. Możesz ją wykonać samodzielnie dla finalnego pliku.

Buildy CommonJS i ES Module zawierają również surowe kontrolki dla `process.env.NODE_ENV` aby sprawdzić w jakim trybie Vue ma pracować. Aby to zrobić należy w konfiguracji wykorzystywanego bundlera zastąpić te zmienne środowiskowe. Zastępując `process.env.NODE_ENV` literałem pozwalającym na minifikację, np: UglifyJS można całkowicie wyeliminować bloki kodu używane podczas dewelopmentu, redukując przy tym rozmiar pliku finalnego.

#### Webpack

Webpack [DefinePlugin](https://webpack.js.org/plugins/define-plugin/):

``` js
var webpack = require('webpack')

module.exports = {
  // ...
  plugins: [
    // ...
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: JSON.stringify('production')
      }
    })
  ]
}
```

#### Rollup

Rollup [rollup-plugin-replace](https://github.com/rollup/rollup-plugin-replace):

``` js
const replace = require('rollup-plugin-replace')

rollup({
  // ...
  plugins: [
    replace({
      'process.env.NODE_ENV': JSON.stringify('production')
    })
  ]
}).then(...)
```

#### Browserify

Dodanie globalnie [envify](https://github.com/hughsk/envify) przekształci Twój build.

``` bash
NODE_ENV=production browserify -g envify -e main.js | uglifyjs -c -m > build.js
```

Zobacz również [Production Deployment Tips](deployment.html).

### Środowiska CSP

Niektóre środowiska, takie jak Google Chrome Apps, wymuszają Content Security Policy (CSP), która zabrania używania `new Function()`. full build bazuje na tej własności, więc jest bezużyteczny w tym przypadku.

Natomiast build untime-only jest w pełni kompatybilny z CSP. Korzystając z niego w połączeniu z [Webpack + vue-loader](https://github.com/vuejs-templates/webpack-simple) lub [Browserify + vueify](https://github.com/vuejs-templates/browserify-simple), twoje templatki będą prekompilowane do funkcji typu `render`, które świetnie współpracują ze środowiskami CSP.

## Dev Build

**Ważne**: pliki znajdujące się na Github w folderze `/dist` są przygotowywane w chwili wypuszczania kolejnej wersji. Aby korzystać z najnowszej wersji Vue musisz przygotować build samemu!

``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
npm install
npm run build
```

## Bower

Z Bower dostępne są tylko buildy UMD.

``` bash
# najnowsza stabilna wersja
$ bower install vue
```

## AMD Module Loaders

Wszystkie buildy UMD mogą być użyte bezpośrednio jako moduły AMD.
