<template>
  <v-container>
    <v-layout justify-center>
      <v-flex xs6 md8 lg5>
        
     <h1>To do App:</h1>
      <div>
    <input type="text" class="todo-input" placeholder="What should we do today ..?" v-model="newTodo" @keyup.enter="addTodo">
 
    <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo-item">
      <div class="todo-item-left">
        <input type="checkbox" class="checkmark" v-model="todo.completed">
        <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{ completed : todo.completed }">{{ todo.title }}</div>
        <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
      </div>
      <div class="remove-item" @click="removeTodo(index)" title="Remove this item">
        &times;
      </div>
    </div>


    <div class="extra-container">
      <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos"> Select All</label></div>
      <div> <b>{{ remaining }}</b> Task(s) left to do</div>
    </div>

    <div class="extra-container">
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
        <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</button>
        <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</button>
      </div>

      <div>
        
        <button v-if="showClearCompletedButton" @click="clearCompleted">Delete Selected</button>
       
      </div>

    </div>
  </div>
  </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
      beforeEditCache: '',
      filter: 'all',
      todos: [
        {
          'id': 1,
          'title': 'Start coding',
          'completed': false,
          'editing': false,
        },
        {
          'id': 2,
          'title': 'Cry because code doesnt work',
          'completed': false,
          'editing': false,
        },
      ]
    }
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining() {
      return this.remaining != 0
    },
    todosFiltered() {
      if (this.filter == 'all') {
        return this.todos
      } else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    },
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length == 0) {
        return
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
        editing: false,
      })
      this.newTodo = ''
      this.idForTodo++
    },
    editTodo(todo) {
      this.beforeEditCache = todo.title
      todo.editing = true
    },
    doneEdit(todo) {
      if (todo.title.trim() == '') {
        todo.title = this.beforeEditCache
      }
      todo.editing = false
    },
    cancelEdit(todo) {
      todo.title = this.beforeEditCache
      todo.editing = false
    },
    removeTodo(index) {
      this.todos.splice(index, 1)
    },
    checkAllTodos() {
      this.todos.forEach((todo) => todo.completed = event.target.checked)
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
}
</script>

<style lang="scss">

.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
  background-color: rgb(100, 247, 63);
  color: rgb(168, 8, 8);
&:focus {
  outline: 0;
}
}

.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.remove-item{
  font-size: 25px;
  cursor: pointer;
  margin-left: 14px;
  color: red;
  &:hover {
    color:rgb(143, 46, 46);
  }
}
.todo-item-left{
  display: flex;
  align-items: center;
}
.todo-item-label {
  padding: 10px;
  cursor: pointer;
  margin-left: 12px;
}
.todo-item-edit {
  font-size: 24px;
  margin-left: 12px;
  color: #2c3e50;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;

  &:focus{
    outline: none;
  }

}

.completed{
  text-decoration: line-through;
  color: rgb(177, 0, 0); 
}

.extra-container{
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid rgb(12, 182, 6);
  padding-top: 14px;
  margin-bottom: 14px;
}
  button {
  font-size: 14px;
  background-color: rgb(224, 226, 214);
  padding: 13px;
  width: 105px;
  
 

  &:hover {
    background: lightgreen;
  }

  &:focus {
    outline: none;
  }
  }
  .active {
    background: lightgreen;
}
  

</style>
