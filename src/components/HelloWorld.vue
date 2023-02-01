<template>
  <div class="container">

    <h2>To Do: ({{ todos.length }})</h2>
    <input type="text" placeholder="Type something and press ENTER" @keypress.enter="addTodo" v-model="inputTodo" />

    <ol>
      <TodoList :todos="todos" @toggle="complete" @remove="remove" />
    </ol>

    <hr />

    <h2>Completed Items: ({{ completed.length }})</h2>

    <ol>
      <TodoList :completed="true" :todos="completed" @toggle="uncomplete" @remove="remove" />
    </ol>

  </div>
</template>

<script>
import TodoList from './TodoList.vue'
export default {
  name: 'HelloWorld',
  components: {
    TodoList
  },
  data() {
    return {
      inputTodo: '',
      todos: [
        { id: 1, text: "Learn about Vue" },
        { id: 2, text: "Learn about Fliplet" },
        { id: 3, text: "Play around in JSFiddle" },
        { id: 4, text: "Show us what you've got" }
      ],
      completed: [],
      controlOnStart: true
    }
  },
  methods: {
    addTodo() {
      if (this.inputTodo.trim())
        this.todos.unshift({ text: this.inputTodo.trim(), id: new Date().getTime() })
      this.inputTodo = ''
    },
    remove(id, completed) {
      if (completed) {
        const index = this.completed.findIndex(el => el.id === id)
        this.completed.splice(index, 1)
      }
      else {
        const index = this.todos.findIndex(el => el.id === id)
        this.todos.splice(index, 1)
      }
    },
    complete(item, index) {
      this.completed.push({ ...item, id: new Date().getTime() })
      this.todos.splice(index, 1)

    },
    uncomplete(item, index) {
      this.todos.push({ ...item, id: new Date().getTime() })
      this.completed.splice(index, 1)

    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
body {
  background: #20262e;
  padding: 20px;
  font-family: Helvetica;

}

.container {
  max-width: 991px;
  margin: 0 auto;
}

#app {
  background: #fff;
  border-radius: 4px;
  padding: 20px;
  transition: all 0.2s;
}

li {
  margin: 8px 0;
  cursor: grab;

}

h2 {
  font-weight: bold;
  margin-bottom: 15px;
}

del {
  color: rgba(0, 0, 0, 0.3);
}

input[type="text"] {
  padding: 10px;
  width: 215px;
}
input[type="checkbox"]{
  cursor: pointer;
}

label {
  display: block;
}

label .remove {
  display: none;
  color: red;
}

label:hover .remove {
  display: inline-block;
  margin-left: 10px;
  color: red;
  opacity: 0.5;
}
</style>
