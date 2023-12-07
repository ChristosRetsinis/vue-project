<script setup>
import { reactive, ref,computed,onMounted,watch } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import ChildComp from './components/ChildComp.vue'
import TheWelcome from './components/TheWelcome.vue'
let id = 0
const counter = reactive({ count: 0 })
//const greeting = ref('Hello from parent')
const msg = ref('from parent')
const titleClass = ref('title')
const pElementRef = ref(null)
const message = ref('Hello World!')
const childMsg = ref('No child msg yet')
const count = ref(0)
const awesome = ref(true)
const todoId = ref(1)
const todoData = ref(null)  
const text = ref('')
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

function onInput(e) {// a v-on handler receives the native DOM event// as the argument.
text.value = e.target.value}
function increment() {count.value++}
async function fetchData() {
  todoData.value = null
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  todoData.value = await res.json()
}

function toggle() {awesome.value = !awesome.value}
function addTodo() {todos.value.push({ id: id++, text: newTodo.value, done: false })
newTodo.value = ''}
function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
onMounted(() => {
  pElementRef.value.textContent = 'mounted!'
})
fetchData()
watch(todoId, fetchData)

</script>

<template>
<p ref="pElementRef">hello</p> 
<header>
  <input :value="text" @input="onInput">
  <p>{{ text }}</p>
    <h1 :class="titleClass">Make me red as hell</h1>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>
  <h1>{{ message }}</h1>
  <p>Count is: {{ counter.count }}</p>
  <p>Todo id: {{ todoId }}</p>
  <button @click="todoId++" :disabled="!todoData">Fetch next todo</button>
  <p v-if="!todoData">Loading...</p>
  <pre v-else>{{ todoData }}</pre>

  <main>
    <TheWelcome />
  </main>
  
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
  <button v-on:click="increment">count is: {{ count }}</button>

  <button @click="toggle">toggle</button>
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
  
  <ChildComp>Message: {{ msg }}</ChildComp>
  <p>{{ childMsg }}</p>
</template>


<style scoped>
header {
  line-height: 1.5;
}
.title {
  color: red;}
.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);

  }
 

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
  .done {
  text-decoration: line-through;
}
 
}
</style>
