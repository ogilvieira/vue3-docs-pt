# Diretivas Embutidas {#built-in-directives}


## v-text {#v-text}

Atualiza o conteúdo de texto do elemento.

- **Espera:** `string`

- **Detalhes**

  `v-text` funciona setando a propriedade [textContent](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent), então irá sobreescrever qualquer conteúdo existente dentro do elemento. Se você precisa atualizar parte do `textContent`, você deverá utilizar [interpolação mustache](/guide/essentials/template-syntax.html#interpolacao-de-texto) ao invés de `v-text`.

- **Exemplo**

  ```vue-html
  <span v-text="msg"></span>
  <!-- a same as -->
  <span>{{msg}}</span>
  ```

- **Veja Também:** [Sintaxe do Modelo de Marcação - Interpolação de Texto](/guide/essentials/template-syntax.html#interpolacao-de-texto)

## v-html {#v-html}

Atualiza o [innerHTML](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML) do elemento.

- **Espera:** `string`

- **Detalhes:**

  Conteúdos de `v-html` são inseridos como HTML plano - A sintaxe de template do Vue não será processada. Se você estiver tentando compôr templates usando `v-html`, tente repensar a solução usando componentes ao invés disso.

  ::: warning Aviso de Segurança
  Renderizar arbitrariamente HTML dinâmico em seu site pode ser muito perigoso pois pode ser facilmente levar a [ataques XSS](https://pt.wikipedia.org/wiki/Cross-site_scripting). Somente use `v-html` com conteúdo seguro e **nunca** com conteúdo fornecido pelo usuário.
  :::

  Em [Componentes de Ficheiro Único](/guire/scaling-up/sfc), estilos `scoped` não serão aplicados ao conteúdo dentro de `v-html`, pois o HTML não é processado pelo compilador de template do Vue. Se você deseja ter o conteúdo de `v-html` com CSS com escopo, você poderá usar [CSS modules](./sfc-css-features.html#css-modules) ou um elemento `<style>` global adicional com alguma estratégia manual de escopo como o BEM.

- **Example:**

  ```vue-html
  <div v-html="html"></div>
  ```

- **Veja Também:** [Template Syntax - Raw HTML](/guide/essentials/template-syntax.html#raw-html)

## v-show {#v-show}

Toggle the element's visibility based on the truthy-ness of the expression value.

- **Espera:** `any`

- **Detalhes**

  `v-show` works by setting the `display` CSS property via inline styles, and will try to respect the initial `display` value when the element is visible. It also triggers transitions when its condition changes.

- **Veja Também:** [Conditional Rendering - v-show](/guide/essentials/conditional.html#v-show)

## v-if {#v-if}

Conditionally render an element or a template fragment based on the truthy-ness of the expression value.

- **Espera:** `any`

- **Detalhes**

  When a `v-if` element is toggled, the element and its contained directives / components are destroyed and re-constructed. If the initial condition is falsy, then the inner content won't be rendered at all.

  Can be used on `<template>` to denote a conditional block containing only text or multiple elements.

  This directive triggers transitions when its condition changes.

  When used together, `v-if` has a higher priority than `v-for`. We don't recommend using these two directives together on one element — see the [list rendering guide](/guide/essentials/list.html#v-for-with-v-if) for details.

- **Veja Também:** [Conditional Rendering - v-if](/guide/essentials/conditional.html#v-if)

## v-else {#v-else}

Denote the "else block" for `v-if` or a `v-if` / `v-else-if` chain.

- **Does not expect expression**

- **Detalhes**

  - Restriction: previous sibling element must have `v-if` or `v-else-if`.

  - Can be used on `<template>` to denote a conditional block containing only text or multiple elements.

- **Exemplo**

  ```vue-html
  <div v-if="Math.random() > 0.5">
    Now you see me
  </div>
  <div v-else>
    Now you don't
  </div>
  ```

- **Veja Também:** [Conditional Rendering - v-else](/guide/essentials/conditional.html#v-else)

## v-else-if {#v-else-if}

Denote the "else if block" for `v-if`. Can be chained.

- **Espera:** `any`

- **Detalhes**

  - Restriction: previous sibling element must have `v-if` or `v-else-if`.

  - Can be used on `<template>` to denote a conditional block containing only text or multiple elements.

- **Exemplo**

  ```vue-html
  <div v-if="type === 'A'">
    A
  </div>
  <div v-else-if="type === 'B'">
    B
  </div>
  <div v-else-if="type === 'C'">
    C
  </div>
  <div v-else>
    Not A/B/C
  </div>
  ```

- **Veja Também:** [Conditional Rendering - v-else-if](/guide/essentials/conditional.html#v-else-if)

## v-for {#v-for}

Render the element or template block multiple times based on the source data.

- **Espera:** `Array | Object | number | string | Iterable`

- **Detalhes**

  The directive's value must use the special syntax `alias in expression` to provide an alias for the current element being iterated on:

  ```vue-html
  <div v-for="item in items">
    {{ item.text }}
  </div>
  ```

  Alternatively, you can also specify an alias for the index (or the key if used on an Object):

  ```vue-html
  <div v-for="(item, index) in items"></div>
  <div v-for="(value, key) in object"></div>
  <div v-for="(value, name, index) in object"></div>
  ```

  The default behavior of `v-for` will try to patch the elements in-place without moving them. To force it to reorder elements, you should provide an ordering hint with the `key` special attribute:

  ```vue-html
  <div v-for="item in items" :key="item.id">
    {{ item.text }}
  </div>
  ```

  `v-for` can also work on values that implement the [Iterable Protocol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterable_protocol), including native `Map` and `Set`.

- **Veja Também:**
  - [List Rendering](/guide/essentials/list.html)

## v-on {#v-on}

Attach an event listener to the element.

- **Shorthand:** `@`

- **Espera:** `Function | Inline Statement | Object (without argument)`

- **Argument:** `event` (optional if using Object syntax)

- **Modifiers:**

  - `.stop` - call `event.stopPropagation()`.
  - `.prevent` - call `event.preventDefault()`.
  - `.capture` - add event listener in capture mode.
  - `.self` - only trigger handler if event was dispatched from this element.
  - `.{keyAlias}` - only trigger handler on certain keys.
  - `.once` - trigger handler at most once.
  - `.left` - only trigger handler for left button mouse events.
  - `.right` - only trigger handler for right button mouse events.
  - `.middle` - only trigger handler for middle button mouse events.
  - `.passive` - attaches a DOM event with `{ passive: true }`.

- **Detalhes**

  The event type is denoted by the argument. The expression can be a method name, an inline statement, or omitted if there are modifiers present.

  When used on a normal element, it listens to [**native DOM events**](https://developer.mozilla.org/en-US/docs/Web/Events) only. When used on a custom element component, it listens to **custom events** emitted on that child component.

  When listening to native DOM events, the method receives the native event as the only argument. If using inline statement, the statement has access to the special `$event` property: `v-on:click="handle('ok', $event)"`.

  `v-on` also supports binding to an object of event / listener pairs without an argument. Note when using the object syntax, it does not support any modifiers.

- **Exemplo:**

  ```vue-html
  <!-- method handler -->
  <button v-on:click="doThis"></button>

  <!-- dynamic event -->
  <button v-on:[event]="doThis"></button>

  <!-- inline statement -->
  <button v-on:click="doThat('hello', $event)"></button>

  <!-- shorthand -->
  <button @click="doThis"></button>

  <!-- shorthand dynamic event -->
  <button @[event]="doThis"></button>

  <!-- stop propagation -->
  <button @click.stop="doThis"></button>

  <!-- prevent default -->
  <button @click.prevent="doThis"></button>

  <!-- prevent default without expression -->
  <form @submit.prevent></form>

  <!-- chain modifiers -->
  <button @click.stop.prevent="doThis"></button>

  <!-- key modifier using keyAlias -->
  <input @keyup.enter="onEnter" />

  <!-- the click event will be triggered at most once -->
  <button v-on:click.once="doThis"></button>

  <!-- object syntax -->
  <button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
  ```

  Listening to custom events on a child component (the handler is called when "my-event" is emitted on the child):

  ```vue-html
  <MyComponent @my-event="handleThis" />

  <!-- inline statement -->
  <MyComponent @my-event="handleThis(123, $event)" />
  ```

- **Veja Também:**
  - [Event Handling](/guide/essentials/event-handling.html)
  - [Components - Custom Events](/guide/essentials/component-basics.html#listening-to-events)

## v-bind {#v-bind}

Dynamically bind one or more attributes, or a component prop to an expression.

- **Shorthand:** `:` or `.` (when using `.prop` modifier)

- **Espera:** `any (with argument) | Object (without argument)`

- **Argument:** `attrOrProp (optional)`

- **Modifiers:**

  - `.camel` - transform the kebab-case attribute name into camelCase.
  - `.prop` - force a binding to be set as a DOM property. <sup class="vt-badge">3.2+</sup>
  - `.attr` - force a binding to be set as a DOM attribute. <sup class="vt-badge">3.2+</sup>

- **Usage:**

  When used to bind the `class` or `style` attribute, `v-bind` supports additional value types such as Array or Objects. See linked guide section below for more details.

  When setting a binding on an element, Vue by default checks whether the element has the key defined as a property using an `in` operator check. If the property is defined, Vue will set the value as a DOM property instead of an attribute. This should work in most cases, but you can override this behavior by explicitly using `.prop` or `.attr` modifiers. This is sometimes necessary, especially when [working with custom elements](/guide/extras/web-components.html#passing-dom-properties).

  When used for component prop binding, the prop must be properly declared in the child component.

  When used without an argument, can be used to bind an object containing attribute name-value pairs.

- **Exemplo:**

  ```vue-html
  <!-- bind an attribute -->
  <img v-bind:src="imageSrc" />

  <!-- dynamic attribute name -->
  <button v-bind:[key]="value"></button>

  <!-- shorthand -->
  <img :src="imageSrc" />

  <!-- shorthand dynamic attribute name -->
  <button :[key]="value"></button>

  <!-- with inline string concatenation -->
  <img :src="'/path/to/images/' + fileName" />

  <!-- class binding -->
  <div :class="{ red: isRed }"></div>
  <div :class="[classA, classB]"></div>
  <div :class="[classA, { classB: isB, classC: isC }]"></div>

  <!-- style binding -->
  <div :style="{ fontSize: size + 'px' }"></div>
  <div :style="[styleObjectA, styleObjectB]"></div>

  <!-- binding an object of attributes -->
  <div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

  <!-- prop binding. "prop" must be declared in the child component. -->
  <MyComponent :prop="someThing" />

  <!-- pass down parent props in common with a child component -->
  <MyComponent v-bind="$props" />

  <!-- XLink -->
  <svg><a :xlink:special="foo"></a></svg>
  ```

  The `.prop` modifier also has a dedicated shorthand, `.`:

  ```vue-html
  <div :someProperty.prop="someObject"></div>

  <!-- equivalent to -->
  <div .someProperty="someObject"></div>
  ```

  The `.camel` modifier allows camelizing a `v-bind` attribute name when using in-DOM templates, e.g. the SVG `viewBox` attribute:

  ```vue-html
  <svg :view-box.camel="viewBox"></svg>
  ```

  `.camel` is not needed if you are using string templates, or pre-compiling the template with a build step.

- **Veja Também:**
  - [Class and Style Bindings](/guide/essentials/class-and-style.html)
  - [Components - Prop Passing Details](/guide/components/props.html#prop-passing-details)

## v-model {#v-model}

Create a two-way binding on a form input element or a component.

- **Espera:** varies based on value of form inputs element or output of components

- **Limited to:**

  - `<input>`
  - `<select>`
  - `<textarea>`
  - components

- **Modifiers:**

  - [`.lazy`](/guide/essentials/forms.html#lazy) - listen to `change` events instead of `input`
  - [`.number`](/guide/essentials/forms.html#number) - cast valid input string to numbers
  - [`.trim`](/guide/essentials/forms.html#trim) - trim input

- **Veja Também:**

  - [Form Input Bindings](/guide/essentials/forms.html)
  - [Component Events - Usage with `v-model`](/guide/components/v-model.html)

## v-slot {#v-slot}

Denote named slots or scoped slots that expect to receive props.

- **Shorthand:** `#`

- **Espera:** JavaScript expression that is valid in a function argument position, including support for destructuring. Optional - only needed if expecting props to be passed to the slot.

- **Argument:** slot name (optional, defaults to `default`)

- **Limited to:**

  - `<template>`
  - [components](/guide/components/slots.html#scoped-slots) (for a lone default slot with props)

- **Exemplo:**

  ```vue-html
  <!-- Named slots -->
  <BaseLayout>
    <template v-slot:header>
      Header content
    </template>

    <template v-slot:default>
      Default slot content
    </template>

    <template v-slot:footer>
      Footer content
    </template>
  </BaseLayout>

  <!-- Named slot that receives props -->
  <InfiniteScroll>
    <template v-slot:item="slotProps">
      <div class="item">
        {{ slotProps.item.text }}
      </div>
    </template>
  </InfiniteScroll>

  <!-- Default slot that receive props, with destructuring -->
  <Mouse v-slot="{ x, y }">
    Mouse position: {{ x }}, {{ y }}
  </Mouse>
  ```

- **Veja Também:**
  - [Components - Slots](/guide/components/slots.html)

## v-pre {#v-pre}

Skip compilation for this element and all its children.

- **Does not expect expression**

- **Detalhes**

  Inside the element with `v-pre`, all Vue template syntax will be preserved and rendered as-is. The most common use case of this is displaying raw mustache tags.

- **Exemplo:**

  ```vue-html
  <span v-pre>{{ this will not be compiled }}</span>
  ```

## v-once {#v-once}

Render the element and component once only, and skip future updates.

- **Does not expect expression**

- **Detalhes**

  On subsequent re-renders, the element/component and all its children will be treated as static content and skipped. This can be used to optimize update performance.

  ```vue-html
  <!-- single element -->
  <span v-once>This will never change: {{msg}}</span>
  <!-- the element have children -->
  <div v-once>
    <h1>comment</h1>
    <p>{{msg}}</p>
  </div>
  <!-- component -->
  <MyComponent v-once :comment="msg"></MyComponent>
  <!-- `v-for` directive -->
  <ul>
    <li v-for="i in list" v-once>{{i}}</li>
  </ul>
  ```

  Since 3.2, you can also memoize part of the template with invalidation conditions using [`v-memo`](#v-memo).

- **Veja Também:**
  - [Data Binding Syntax - interpolations](/guide/essentials/template-syntax.html#interpolacao-de-texto)
  - [v-memo](#v-memo)

## v-memo <sup class="vt-badge" data-text="3.2+" /> {#v-memo}

- **Espera:** `any[]`

- **Detalhes**

  Memoize a sub-tree of the template. Can be used on both elements and components. The directive expects a fixed-length array of dependency values to compare for the memoization. If every value in the array was the same as last render, then updates for the entire sub-tree will be skipped. For example:

  ```vue-html
  <div v-memo="[valueA, valueB]">
    ...
  </div>
  ```

  When the component re-renders, if both `valueA` and `valueB` remain the same, all updates for this `<div>` and its children will be skipped. In fact, even the Virtual DOM VNode creation will also be skipped since the memoized copy of the sub-tree can be reused.

  It is important to specify the memoization array correctly, otherwise we may skip updates that should indeed be applied. `v-memo` with an empty dependency array (`v-memo="[]"`) would be functionally equivalent to `v-once`.

  **Usage with `v-for`**

  `v-memo` is provided solely for micro optimizations in performance-critical scenarios and should be rarely needed. The most common case where this may prove helpful is when rendering large `v-for` lists (where `length > 1000`):

  ```vue-html
  <div v-for="item in list" :key="item.id" v-memo="[item.id === selected]">
    <p>ID: {{ item.id }} - selected: {{ item.id === selected }}</p>
    <p>...more child nodes</p>
  </div>
  ```

  When the component's `selected` state changes, a large amount of VNodes will be created even though most of the items remained exactly the same. The `v-memo` usage here is essentially saying "only update this item if it went from non-selected to selected, or the other way around". This allows every unaffected item to reuse its previous VNode and skip diffing entirely. Note we don't need to include `item.id` in the memo dependency array here since Vue automatically infers it from the item's `:key`.

  :::warning
  When using `v-memo` with `v-for`, make sure they are used on the same element. **`v-memo` does not work inside `v-for`.**
  :::

  `v-memo` can also be used on components to manually prevent unwanted updates in certain edge cases where the child component update check has been de-optimized. But again, it is the developer's responsibility to specify correct dependency arrays to avoid skipping necessary updates.

- **Veja Também:**
  - [v-once](#v-once)

## v-cloak {#v-cloak}

Used to hide un-compiled template until it is ready.

- **Does not expect expression**

- **Detalhes**

  **This directive is only needed in no-build-step setups.**

  When using in-DOM templates, there can be a "flash of un-compiled templates": the user may see raw mustache tags until the mounted component replaces them with rendered content.

  `v-cloak` will remain on the element until the associated component instance is mounted. Combined with CSS rules such as `[v-cloak] { display: none }`, it can be used to hide the raw templates until the component is ready.

- **Exemplo:**

  ```css
  [v-cloak] {
    display: none;
  }
  ```

  ```vue-html
  <div v-cloak>
    {{ message }}
  </div>
  ```

  The `<div>` will not be visible until the compilation is done.
