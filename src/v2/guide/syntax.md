---
title: Składnia szablonów
type: guide
order: 4
---

Vue.js korzysta ze składni szablonów bazującej na składni HTML-a co pozwala na deklaratywne bindowanie renderowanego DOMu do danych instancji Vue.js. Wszystkie szablony Vue.js zawierają prawidłowy HTML który może być parsowany przez przegladrki zgodne ze standardami i parsery HTML.

Vue korzystając z funkcji renderujących kompiluje szablony w tle do wirtualnego DOM. W połączeniu z systemem reaktywności potrafi inteligentnie obliczyć minimalną liczbę składników do ponownego renderowania aby zastosować minimalną ilość manipulacji DOM po zmianie stanu aplikacji.

Jeżeli koncepcja wirtualnego DOM, nie jest Ci obca i wolisz używać czystego JavaScript możesz pisać bezpośrednio [funkcje renderujące ](render-function.html) zamiast szablonów, z opcjonalnym wsparciem JSX.

## Interpolacje

### Tekst

Najprostszym sposobem bindowania danych jest interpolacja tekstu z użyciem składni "Mustache" (podwójne nawiasy klamrowe).

``` html
<span>Wiadomość: {{ msg }}</span>
```

Tag "Mustache" zostanie zastąpiony wartością `msg` pobraną z obiektu data. Będzie również aktualizował w przypadku zmiany wartości `msg` w obiekcie data.

Jest również mozliwość wykonania jednorazowej interpolacji tekstu. Korzystając z dyrektywy [v-once](../api/#v-once). Wówczas dane nie będa się aktualizować, ale pamiętaj iż ta dyrektywa wpływa na wszystkie elementy w tym węźle:

``` html
<span v-once>This will never change: {{ msg }}</span>
```

### Surowy HTML

Podwójne nawiasy klamrowe interpretują dane jako tekst nie jako HTML. Aby wstawić HTML trzeba użyć dyrektywy `v-html`:

``` html
<p>Nawiasy klamrowe: {{ rawHtml }}</p>
<p>Dyrektywa v-html: <span v-html="rawHtml"></span></p>
```

{% raw %}
<div id="example1" class="demo">
  <p>Nawiasy klamrowe: {{ rawHtml }}</p>
  <p>Dyrektywa v-html: <span v-html="rawHtml"></span></p>
</div>
<script>
new Vue({
  el: '#example1',
  data: function () {
  	return {
  	  rawHtml: '<span style="color: red">Ten tekst powinien być czerwony</span>'
  	}
  }
})
</script>
{% endraw %}

Zawartość tagu `span` zostanie zastąpiona zawartością `rawHtml` i zinterpretowana jako HTML - bindowane dane zostaną zignorowane.
The contents of the `span` will be replaced with the value of the `rawHtml` property, interpreted as plain HTML - data bindings are ignored. Pamiętaj jednak, że Vue nie jest silnikiem szablonów bazującym na stringach, więc nie możesz konstruować szablonów bazując na `v-html`. Do tego służa komponenty, jako podstawowy element nadający się do ponownego wykorzystania i projektowania interfejsu użytkownika.

<p class="tip">Dynamicznie renderowany arbitralny HTML może dawać mozliwość do wykorzystania luki w zabezpieczeniach [XSS vulnerabilities](https://en.wikipedia.org/wiki/Cross-site_scripting). Bezpieczeństwo zapewnia wykorzystanie interpolacji HTML na własnej treści, **nigdy** na treści dostarczanej przez użytkownika.</p>

### Atrybuty

Nawiasy klamrowe nie moga być wykorzystywane wewnątrz atrybutów HTML. Jeżeli potrzebujesz aby atrybut był dynamiczny korzystaj z dyrektywy [v-bind](../api/#v-bind):

``` html
<div v-bind:id="dynamiczneId"></div>
```

W przypadku wartości logicznych, których samo istnienie implikuję `true`, `v-bind` działa nieco inaczej:

``` html
<button v-bind:disabled="isButtonDisabled">Button</button>
```

Jeżeli `isButtonDisabled` ma wartość `null`, `undefined` lub `false`, atrybut `disabled` nie będzię uwzględniony w renderowanym elemencie `<button>`.

### Korzystanie z wyrażeń JavaScript

Jak dotąd w szablonach bindowaliśmy tylko proste właściwości. Ale Vue.js daje możliwość wykorzystania całego potencjały JavaScript wewnątrz bindowania:

``` html
{{ numer + 1 }}

{{ ok ? 'TAK' : 'NIE' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

Wyrażenia te zostana zinterpretowane jako JavaScript o zasięgu instancji Vue, która jest ich właścicielem.

Jedynym ograniczeniem jest, że bindowanie może zawierac **tylko jedno wyrażenie**, w związku z tym poniższy kod **nie zadziała**:

``` html
<!-- to jest deklaracja nie wyrażenie: -->
{{ var a = 1 }}

<!-- tu trzeba uzyć potrójnego wyrażenia warunkowego, w takiej formie nie zadziała: -->
{{ if (ok) { return message } }}
```

<p class="tip">Szablony są izolowane i mają dostęp do globalnych obiektów z białej listy takich jak `Math` i `Date`. Nie należy dawać w szablonach użtkownikowi dostępu do obiektów globalnych.</p>

## Dyrektywy

Dyrektywy są specjalnymi atrybutami z prefiksem `v-`. Wartości atrybutu dyrektywy powinny być **pojedyńczym wyrażeniem JavaScript** (za wyjątkiem `v-for` o czym będzie nieco później). Zadaniem dyrektywy jest reaktywne aktualizowanie DOM przy zmianach wartości wyrażenia. Przyjrzyj się poniżeszemu wyrażeniu:

``` html
<p v-if="seen">Teraz mnie widać</p>
```

Tutaj dyrektywa `v-if` dodaje/usuwa element `<p>` w zalezności od prawdziwości własności `seen`.

### Argumenty

Niektóre dyrektywy przyjmują argumenty, oznaczane są dwukropkiem po nazwie. Na poniższym przykładzie dyrektywa `v-bind` użyta do reaktywnego aktualizowania atrybutu HTML:

``` html
<a v-bind:href="url"> ... </a>
```

W tym przypadku `href` jest argumentem, który przekazuje dyrektywie `v-bind` polecenie bindowania do atrybutu `href` wartości `url`.

Innym przykładem jest dyrektywa `v-on` nasłuchująca zdarzeń w DOM:

``` html
<a v-on:click="zrobCos"> ... </a>
```

Tutaj argument jest nazwą zdarzenia którego oczekuje. Będzie o tym bardziej szczegółowo w dalszej części.

### Modyfikatory

Modyfikatory są specjalnymi postfiksami zapisywanymi po kropce. Wskazują, że dyrektywa ma być bindowana w określony sposób. Na przykład modyfikator `.prevent` informuje dyrektywę `v-on`, że ma wywołać funkcje `event.preventDefault()` na wywołanym zdarzeniu:

``` html
<form v-on:submit.prevent="onSubmit"> ... </form>
```

W dalszej części będzie więcej przykładów modyfikatorów [dla `v-on`](events.html#Event-Modifiers) i [dla `v-model`](forms.html#Modifiers).

## Skróty

Prefix `v-` wizualnie identyfikuje atrybuty Vue w szablonach. Jest to użyteczne podczas wykorzystywania Vue.js do dodawania dynamicznych elementów w utworzonym juz kodzie, ale nadużywane może zmniejszać czytelność kodu. Podczas budowania jedstronnicowej aplikacji [SPA](https://en.wikipedia.org/wiki/Single-page_application), gdzie Vue.js zarządza każdym szablonem staje się również mniej potrzebne. Dlatego Vue.js obsługuje specjalne skróty dla dwóch najpopularniejszych dyrektyw `v-bind` and `v-on`:

### Skrót dla `v-bind`

``` html
<!-- składnia pełna -->
<a v-bind:href="url"> ... </a>

<!-- składnia skrócona -->
<a :href="url"> ... </a>
```

### Skrót dla `v-on`

``` html
<!-- składnia pełna -->
<a v-on:click="zrobCos"> ... </a>

<!-- składnia skrócona -->
<a @click="zrobCos"> ... </a>
```

Może to wyglądać nieco inaczej od normalnego HTMLa, ale znaki `:` i `@` w przypadku atrybutów są prawidłowo parsowane przez wszystkie przeglądarki wspierające Vue.js. Ponadto nie są renderowane w ostatecznym kodzie. Składnia skrócona jest opcjonalna, ale z pewnością w miarę nauki docenisz jej zalety.
