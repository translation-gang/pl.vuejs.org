---
title: Installation
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

<p class="tip">W trakcie prac na aplikacją nie używaj wesji produkcyjnej, ponieważ nie zawiera obsługi napopularniejszych błędów przez ostrzeżenia!</p>

<div id="downloads">
<a class="button" href="/js/vue.js" download>Wersja developerska</a><span class="light info">zawiera wszystkie ostrzeżenia i tryb debug</span>

<a class="button" href="/js/vue.min.js" download>Wersja produkcyjna</a><span class="light info">Pozbawiona ostrzeżeń i zminifikowana, {{gz_size}}KB min+gzip</span>
</div>

### CDN

Zalecamy załączanie z linku: [https://cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue), link zawiera najnowszą wersję, zaraz po publikacji na npm. Możesz również zajrzeć do źródła paczki npm na: [https://cdn.jsdelivr.net/npm/vue/](https://cdn.jsdelivr.net/npm/vue/).

Dostępne również [unpkg](https://unpkg.com/vue) oraz [cdnjs](https://cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.js) (cdnjs potrzebuje czasu na synchronizację, dlatego najnowsze wersja może nie być tu dostępna od razu).

## NPM

Podczas budowy dużych aplikacji z Vue zalecamy korzystanie z NPM. Dobrze współpracuje z [Webpack](https://webpack.js.org/) i [Browserify](http://browserify.org/). Vue zapewnia również dodatkowe narzedzia do authoringu [Single File Components](single-file-components.html).

``` bash
# Najnowsza stabilna wersja
$ npm install vue
```

## CLI

Vue.js zapewnia [oficjalne CLI](https://github.com/vuejs/vue-cli) do szybkiej budowy szkieletu jednostronnicowej aplikacji. Dzięki temu dostajesz zestaw narzędzi do nowoczesnej pracy z frontendem. It provides batteries-included build setups for a modern frontend workflow. Wystarczy kilka minut aby uruchomić takie funkcje jak hot-reload, lint-on-save czy production-ready:

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

<p class="tip">The CLI assumes prior knowledge of Node.js and the associated build tools. If you are new to Vue or front-end build tools, we strongly suggest going through <a href="./">the guide</a> without any build tools before using the CLI.</p>

## Explanation of Different Builds

In the [`dist/` directory of the NPM package](https://cdn.jsdelivr.net/npm/vue/dist/) you will find many different builds of Vue.js. Here's an overview of the difference between them:

| | UMD | CommonJS | ES Module |
| --- | --- | --- | --- |
| **Full** | vue.js | vue.common.js | vue.esm.js |
| **Runtime-only** | vue.runtime.js | vue.runtime.common.js | vue.runtime.esm.js |
| **Full (production)** | vue.min.js | - | - |
| **Runtime-only (production)** | vue.runtime.min.js | - | - |

### Terms

- **Full**: builds that contains both the compiler and the runtime.

- **Compiler**: code that is responsible for compiling template strings into JavaScript render functions.

- **Runtime**: code that is responsible for creating Vue instances, rendering and patching virtual DOM, etc. Basically everything minus the compiler.

- **[UMD](https://github.com/umdjs/umd)**: UMD builds can be used directly in the browser via a `<script>` tag. The default file from jsDelivr CDN at [https://cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue) is the Runtime + Compiler UMD build (`vue.js`).

- **[CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1)**: CommonJS builds are intended for use with older bundlers like [browserify](http://browserify.org/) or [webpack 1](https://webpack.github.io). The default file for these bundlers (`pkg.main`) is the Runtime only CommonJS build (`vue.runtime.common.js`).

- **[ES Module](http://exploringjs.com/es6/ch_modules.html)**: ES module builds are intended for use with modern bundlers like [webpack 2](https://webpack.js.org) or [rollup](https://rollupjs.org/). The default file for these bundlers (`pkg.module`) is the Runtime only ES Module build (`vue.runtime.esm.js`).

### Runtime + Compiler vs. Runtime-only

If you need to compile templates on the client (e.g. passing a string to the `template` option, or mounting to an element using its in-DOM HTML as the template), you will need the compiler and thus the full build:

``` js
// this requires the compiler
new Vue({
  template: '<div>{{ hi }}</div>'
})

// this does not
new Vue({
  render (h) {
    return h('div', this.hi)
  }
})
```

When using `vue-loader` or `vueify`, templates inside `*.vue` files are pre-compiled into JavaScript at build time. You don't really need the compiler in the final bundle, and can therefore use the runtime-only build.

Since the runtime-only builds are roughly 30% lighter-weight than their full-build counterparts, you should use it whenever you can. If you still wish to use the full build instead, you need to configure an alias in your bundler:

#### Webpack

``` js
module.exports = {
  // ...
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js' // 'vue/dist/vue.common.js' for webpack 1
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

Add to your project's `package.json`:

``` js
{
  // ...
  "browser": {
    "vue": "vue/dist/vue.common.js"
  }
}
```

### Development vs. Production Mode

Development/production modes are hard-coded for the UMD builds: the un-minified files are for development, and the minified files are for production.

CommonJS and ES Module builds are intended for bundlers, therefore we don't provide minified versions for them. You will be responsible for minifying the final bundle yourself.

CommonJS and ES Module builds also preserve raw checks for `process.env.NODE_ENV` to determine the mode they should run in. You should use appropriate bundler configurations to replace these environment variables in order to control which mode Vue will run in. Replacing `process.env.NODE_ENV` with string literals also allows minifiers like UglifyJS to completely drop the development-only code blocks, reducing final file size.

#### Webpack

Use Webpack's [DefinePlugin](https://webpack.js.org/plugins/define-plugin/):

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

Use [rollup-plugin-replace](https://github.com/rollup/rollup-plugin-replace):

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

Apply a global [envify](https://github.com/hughsk/envify) transform to your bundle.

``` bash
NODE_ENV=production browserify -g envify -e main.js | uglifyjs -c -m > build.js
```

Also see [Production Deployment Tips](deployment.html).

### CSP environments

Some environments, such as Google Chrome Apps, enforce Content Security Policy (CSP), which prohibits the use of `new Function()` for evaluating expressions. The full build depends on this feature to compile templates, so is unusable in these environments.

On the other hand, the runtime-only build is fully CSP-compliant. When using the runtime-only build with [Webpack + vue-loader](https://github.com/vuejs-templates/webpack-simple) or [Browserify + vueify](https://github.com/vuejs-templates/browserify-simple), your templates will be precompiled into `render` functions which work perfectly in CSP environments.

## Dev Build

**Important**: the built files in GitHub's `/dist` folder are only checked-in during releases. To use Vue from the latest source code on GitHub, you will have to build it yourself!

``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
npm install
npm run build
```

## Bower

Only UMD builds are available from Bower.

``` bash
# latest stable
$ bower install vue
```

## AMD Module Loaders

All UMD builds can be used directly as an AMD module.
