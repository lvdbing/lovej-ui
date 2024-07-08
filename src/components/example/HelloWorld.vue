<script>
import ChildComp from './ChildComp.vue'

let id = 0
export default {
  data() {
    return {
      // 1. 声明式渲染
      message: 'Hello World!',
      titleClass: 'title',
      titleClass2: 'title2',
      count: 0,
      text: '',
      awesome: true,
      newTodo: '',
      todos: [
        { id: id++, text: "Learn HTML", done: true },
        { id: id++, text: "Learn JavaScript", done: true },
        { id: id++, text: "Learn Vue", done: false },
      ],
      hideCompleted: false,
      todoId: 1,
      todoData: null,
      greeting: "Hello from parent",
      childMsg: 'No child msg yet',
    }
  },
  computed: {
    filteredTodos() {
      return this.hideCompleted ? this.todos.filter((t)=>!t.done) : this.todos
    }
  },
  methods: {
    increment() {
      this.count++
    },
    onInput(e) {
      this.text = e.target.value
    },
    toggle() {
      this.awesome = !this.awesome;
    },
    addTodo() {
      this.todos.push({id: id++, text: this.newTodo})
      this.newTodo = ''
    },
    removeTodo(todo) {
      this.todos = this.todos.filter(x => x!=todo)
    },
    async fetchData() {
      this.todoData = null
      const res = await fetch(
        `https://jsonplaceholder.typicode.com/todos/${this.todoId}`
      )
      this.todoData = await res.json()
    },
  },
  mounted() {
    // 此时组件已经挂载。
    this.$refs.pElementRef.textContent = "mounted!"
    this.fetchData()
  },
  watch: {
    count(newCount) {
      console.log(`new count is: ${newCount}`)
    },
    todoId() {
      this.fetchData()
    }
  },
  components: {
    ChildComp
  }
}
</script>

<template>
  <!-- 1. 声明式渲染 -->
  <h1>{{ message }}</h1>
  <h1>{{ message.split('').reverse().join('') }}</h1>

  <!-- 2. Attribute绑定 -->
  <h1 v-bind:class="titleClass">Make me red</h1>
  <h1 :class="titleClass2">Make me blue</h1>

  <!-- 3. 事件监听 -->
  <button v-on:click="increment">Count: {{ count }}</button>
  <button @click="increment">Count is: {{ count }}</button>

  <!-- 4. 表单绑定 -->
  <input :value="text" @input="onInput"/>
  <input v-model="text"/>
  <p>{{ text }}</p>

  <!-- 5. 条件渲染 -->
  <button @click="toggle">Toggle</button>
  <h1 v-if="awesome">This is awesome!</h1>
  <h1 v-else>Oh no!</h1>

  <!-- 6. 列表渲染 -->
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" required placeholder="new Todo"/>
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      <input type="checkbox" v-model="todo.done"/>
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>

  <!-- 7. 计算属性 computed -->
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done"/>
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : "Hide completed" }}
  </button>

  <!-- 
    8. 生命周期和模版引用
      mounted钩子-组件已挂载
      ref模版引用 
  -->
  <p ref="pElementRef">Hello</p>

  <!-- 9. 侦听器 -->
   <p>Todo id: {{ todoId }}</p>
   <button @click="todoId++" :disabled="!todoData">Fetch next todo</button>
   <p v-if="!todoData">Loading...</p>
   <pre v-else>{{ todoData }}</pre>

   <!-- 10. 组件 -->
   <ChildComp />

   <!-- 11. Props -->
   <ChildComp :parentMsg="greeting" />

   <!-- 12. Emits -->
   <ChildComp parentMsg="hello from parent again" @response="(msg) => childMsg=msg"/>
   <p>{{ childMsg }}</p>

   <!-- 13. 插槽 -->
    <ChildComp>
      This is some slot content!
      Message: {{ greeting }}
    </ChildComp>

    <!-- Element Plus -->
    <el-button>Default</el-button>
    <el-button type="primary">Primary</el-button>
    <el-button type="success">Success</el-button>
    <el-button type="info">Info</el-button>
    <el-button type="warning">Warning</el-button>
    <el-button type="danger">Danger</el-button>
</template>

<style>
.title {
  color: red;
}
.title2 {
  color: blue;
}
.done {
  text-decoration: line-through;
}
</style>


<!-- <script setup lang="ts">
defineProps<{
  msg: string
}>()
</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <h3>
      You’ve successfully created a project with
      <a href="https://vitejs.dev/" target="_blank" rel="noopener">Vite</a> +
      <a href="https://vuejs.org/" target="_blank" rel="noopener">Vue 3</a>. What's next?
    </h3>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style> -->
