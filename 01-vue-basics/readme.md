# Vue basics

source : [https://vuejs.org/tutorial/](https://vuejs.org/tutorial/)

## Getting Started

```html
<!-- App.vue -->
<template>
  <h1>Hello World!</h1>
</template>
```

## Declarative rendering

```html
<!-- App.vue -->
<script>
import { ref, reactive } from "vue";

// component logic
// declare some reactive state here.

const counter = reactive({ count: 0 });
const message = ref("Hello World!");

counter.counter++;
message.value = "Changed";
</script>

<template>
  <!-- degnahC -->
  <h1>{{ message.split("").reverse().join("") }}</h1>
  <!-- Count is: 0 -->
  <p>count is: {{ counter.count }}</p>
</template>
```

learn more: [Guide - Reactivity Fundamentals](https://vuejs.org/guide/essentials/reactivity-fundamentals.html)

## Attribute Bindings

bind an attribute to a dynamic value

concept :

- `<template>` as semantic html tag
- pass reference as string value on attribute

```html
<!-- App.vue -->
<script setup>
import { ref } from 'vue'

const titleClass = ref('title')
</script>

<template>
  <!-- add dynamic class binding here -->
  <h1 v-bind:class="titleClass">Make me red</h1>
  <h1 :class="titleClass">Make me red</h1> <!-- ":class" is shorhand for "v-bind:class" -->
  <!-- ":class" & ":id" also available -->
</template>

<style>
.title {
  color: red;
}
</style>
```

learn more: [Guide - Template Syntax](https://vuejs.org/guide/essentials/template-syntax.html)

## Event Listeners

using `v-on` directive, shorthand syntax: `@`

```html
<script setup>
import { ref } from 'vue'

const count = ref(0)
const increment = () => {
  // update component state
  count.value++
}
</script>

<template>
  <button @click="increment">count is: {{ count }}</button>
</template>
```

learn more : [Guide - Event Handling](https://vuejs.org/guide/essentials/event-handling.html)

## Form Bindings

two-way bindings on form input elements:

```html
<script setup>
import { ref } from 'vue'

const text = ref('')

function onInput(e) {
  text.value = e.target.value
}
</script>

<template>
  <!--  :value="text" refer to const text = ref(''). Reactive  -->
  <input :value="text" @input="onInput" placeholder="Type here">
  <p>{{ text }}</p>
</template>
```

```html
<script setup>
import { ref } from 'vue'

const text = ref('')
</script>

<template>
  <!--  v-model automatically syncs the <input>'s value with the bound state  -->
  <input v-model="text" placeholder="Type here">
  <p>{{ text }}</p>
</template>
```

learn more: [Guide - Form Input Bindings](https://vuejs.org/guide/essentials/forms.html)
