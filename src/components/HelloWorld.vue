<template>
  <div>
    <h2>To do: ({{todos.length}})</h2>
    <input
      type="text"
      placeholder="Type something and press ENTER"
      @keypress.enter="addTodo"
      v-model="inputTodo"
    />

    <ol>
      <draggable
        class="dragArea list-group"
        :list="todos"
        :clone="clone"
        :group="{ name: 'people', pull: pullFunction }"
        @start="start"
      >
        <li v-for="(item, index) in todos" :key="item.text">
          <label>
            <input type="checkbox" />
            <span @click="complete(item, index)">{{ item.text }}</span>
            <a href="#" class="remove" @click="remove(item.id)">Remove</a>
          </label>
        </li>
      </draggable>
    </ol>

    <hr />
    <h2>Completed items: ({{completed.length}})</h2>
    <ol>
      <draggable
        class="dragArea list-group"
        :list="completed"
        :clone="clone"
        :group="{ name: 'people', pull: pullFunction }"
        @start="start"
      >
        <li v-for="(item, index) in completed" :key="item.text+item.id">
          <label>
            <input checked type="checkbox" />
            <span @click="uncomplete(item, index)">{{ item.text }}</span>
            <a href="#" class="remove" @click="remove(item.id)">Remove</a>
          </label>
        </li>
      </draggable>
    </ol>
  </div>
</template>

<script>
import draggable from "vuedraggable";

export default {
  name: 'HelloWorld',
   components: {
    draggable
  },
 data() {
   return{
     inputTodo: '',
      todos: [
      {id: 1, text: "Learn about Vue" },
      {id: 2, text: "Learn about Fliplet" },
      {id: 3, text: "Play around in JSFiddle" },
      {id: 4, text: "Show us what you've got" }
    ],
     completed: [],
     controlOnStart: true
   }
  },
  methods: {
    addTodo(){
      if(this.inputTodo.trim())
      this.todos.unshift({text: this.inputTodo.trim(), id: new Date().getTime()})
      this.inputTodo = ''
    },
    remove(id) {
      const index = this.list.findIndex(el=>el.id===id)
      this.list.splice(index, 1)
    },
      pullFunction() {
      return this.controlOnStart ? "clone" : true;
    },
    start({ originalEvent }) {
      this.controlOnStart = originalEvent.ctrlKey;
    },
       clone(obj) {
      return { ...obj, id: new Date().getTime() };
    },
    complete(item, index){
      this.completed.push({ ...item, id: new Date().getTime() })
      this.todos.splice(index, 1)

},
     uncomplete(item, index){
      this.todos.push({ ...item, id: new Date().getTime() })
      this.completed.splice(index, 1)

}



  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body {
  background: #20262e;
  padding: 20px;
  font-family: Helvetica;
}

#app {
  background: #fff;
  border-radius: 4px;
  padding: 20px;
  transition: all 0.2s;
}

li {
  margin: 8px 0;
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
  width: 200px;
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
