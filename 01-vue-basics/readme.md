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

## Conditional Rendering

```html
<script setup>
import { ref } from 'vue'

const awesome = ref(true)

function toggle() {
  awesome.value = !awesome.value
}
</script>

<template>
  <button @click="toggle">toggle</button>
  <!-- if value of awesome is truthy, render this -->
  <h1 v-if="awesome">Vue is awesome!</h1>
  <!-- if value of awesome is falsy, render this --> 
  <h1 v-else>Oh no 😢</h1>
</template>
```

Learn more : [Guide - Conditional Rendering](https://vuejs.org/guide/essentials/conditional.html)

## List Rendering

```html
<script setup>
import { ref } from 'vue'

// give each todo a unique id
let id = 0

const newTodo = ref('')
const todos = ref([
  { id: id++, text: 'Learn HTML' },
  { id: id++, text: 'Learn JavaScript' },
  { id: id++, text: 'Learn Vue' }
])

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button>Add Todo</button>    
  </form>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      {{ todo.text }}
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
</template>
```

learn more:

- [Special Key Attribute](https://vuejs.org/api/built-in-special-attributes.html#key)
- [Mutating methods](https://stackoverflow.com/questions/9009879/which-javascript-array-functions-are-mutating)
- [Guide - List Rendering](https://vuejs.org/guide/essentials/list.html)

## Computed Property

```html
<<script setup>
import { ref, computed } from 'vue'

let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([
  { id: id++, text: 'Learn HTML', done: true },
  { id: id++, text: 'Learn JavaScript', done: true },
  { id: id++, text: 'Learn Vue', done: false }
])

const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
</>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
  </button>
</template>

<style>
.done {
  text-decoration: line-through;
}
</style>
```

## Lifecycle and Template Refs

there will be cases where we need to manually work with the DOM.

Learn more: [Guide - Special `ref` attribute](https://vuejs.org/api/built-in-special-attributes.html#ref)

```html
<script setup>
import { ref } from 'vue'

// Notice the ref is initialized with null value. This is because the element doesn't exist yet when <script setup> is executed.
const pElementRef = ref(null)
</script>

<template>
  <!-- Hello, not null -->
  <p ref="pElementRef">hello</p>
  <!-- because The template ref is only accessible after the component is mounted -->
</template>
```

To run code after mount, we can use the onMounted() function:

```html
<script setup>
import { ref, onMounted } from 'vue'

const pElementRef = ref(null)

onMounted(() => {
  pElementRef.value.textContent = 'mounted!'
})
</script>

<template>
  <p ref="pElementRef">hello</p>
</template>
```

lifecycle hook - it allows us to register a callback to be called at certain times of the component's lifecycle. There are other hooks such as onUpdated and onUnmounted.

learn more: [Vue - Lifecycle Diagram](https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram)

## Wathcers

```html
<script setup>
import { ref, watch } from 'vue'

const todoId = ref(1)
const todoData = ref(null)

async function fetchData() {
  todoData.value = null
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  todoData.value = await res.json()
}

// first run
fetchData()

// directly watch a ref and the callback gets fired whenever count's value changes.
watch(todoId, fetchData)
</script>

<template>
  <p>Todo id: {{ todoId }}</p>
  <button @click="todoId++">Fetch next todo</button>
  <p v-if="!todoData">Loading...</p>
  <pre v-else>{{ todoData }}</pre>
</template>
```

Learn More: [Guide - Watcher](https://vuejs.org/guide/essentials/watchers.html)

## Components

```html
<!-- ChildComp.vue -->
<template>
  <h2>A Child Component!</h2>
</template>
```

```html
<!-- App.vue -->
<script setup>
import ChildComp from './ChildComp.vue'
</script>

<template>
  <!-- render child component -->
  <ChildComp />
</template>
```

## Props

```html
<script setup>
const props = defineProps({
  msg: String
})
</script>

<template>
  <h2>{{ msg || 'No props passed yet' }}</h2>
</template>
```

```html
<script setup>
import { ref } from 'vue'
import ChildComp from './ChildComp.vue'

const greeting = ref('Hello from parent')
</script>

<template>
  <ChildComp :msg="greeting"/>
</template>
```

## Emits

child component can also emit events to the parent

```html
<script setup>
const emit = defineEmits(['response'])

emit('response', 'hello from child')
</script>

<template>
  <h2>Child component</h2>
</template>
```

```html
<script setup>
import { ref } from 'vue'
import ChildComp from './ChildComp.vue'

const childMsg = ref('No child msg yet')
</script>

<template>
  <ChildComp @response="(msg) => childMsg = msg" />
  <p>{{ childMsg }}</p>
</template>
```

## Slots

Content inside the `<slot>` outlet will be treated as "fallback" content:
it will be displayed if the parent did not pass down any slot content:

```html
<template>
  <slot>Fallback content</slot>
</template>
```

```html
<script setup>
import { ref } from 'vue'
import ChildComp from './ChildComp.vue'

const msg = ref('from parent')
</script>

<template>
  <ChildComp>Message: {{ msg }}</ChildComp>
</template>
```

## You Did It

Next:

- Set up a real Vue project on your machine by following the [Quick Start](https://vuejs.org/guide/quick-start.html).
- Go through the [Main Guide](https://vuejs.org/guide/essentials/application.html), which covers all the topics we learned so far in greater details, and much more.
- Check out some more practical [Examples](https://vuejs.org/examples/#hello-world).
