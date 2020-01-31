<template>
  <div class="container">
    <input type="text" class="form-control" @keyup.enter="addTodo" v-model="todoData">

      <ul class="list-group">
        <li v-for="(todo , index) in todosFiltered" v-bind:key="todo" class="list-group-item">
          <div class="form-check">
            <input class="form-check-input" type="checkbox" value="" id="defaultCheck1" v-model="todo.complete">

            <input v-focus v-if="todo.editing" type="text" v-model="todo.title" @blur="updateTodo(todo)" @keyup.enter="updateTodo(todo)"  @keyup.esc="cancelTodo(todo)">

            <label v-else @dblclick="editTodo(todo)" :class="{ completed: todo.complete }">{{ todo.title }}</label>

            <button type="button" class="close" aria-label="Close" @click="removeTodo(index)">
              <span aria-hidden="true">&times;</span>
            </button>

          </div>

        </li>
      </ul>

      <div class="">
        <p> {{ remaining }} {{ plural }} left </p>

        <p>
          <input class="form-check-input" type="checkbox" :checked="!anyRemaining" @change="checkedAllTodo(todo)">
          Check All
        </p>

        <button type="button" :class="{ selected: visibility == 'all' }" @click="allTodos">All</button>
        <button type="button" :class="{ selected: visibility == 'active' }" @click="activeTodos">Active</button>
        <button type="button" :class="{ selected: visibility == 'complete' }" @click="completeTodos">Completed</button>
        <br>
        <button type="button" v-show="todos.length > remaining" @click="clearCompleteTodos">Clear Completed</button>



      </div>




  </div>
</template>

<script type="text/javascript">


export default {
  name:'toDo',
  data() {
    return{
      msg : 'hi',
      idForTodo: 1,
      todoData:'',
      todos:[],
      display: 'none',
      beforeEditCache: '',
      visibility: 'all',

    }
  },



  directives: {
  focus: {
    // directive definition
    inserted: function (el) {
      el.focus()
    }
  }
},


  methods:{


    addTodo:function(){

      this.todos.push({
        id: this.idForTodo,
        title: this.todoData,
        complete: false,
        editing : false,
      }),
      this.todoData = ""
      this.idForTodo++
    },

    removeTodo: function(index){
      this.todos.splice(index, 1)
    },


    editTodo: function(todo){
      this.beforeEditCache = todo.title
      todo.editing = true
    },


    updateTodo: function(todo){
      todo.editing  = false
    },


    cancelTodo: function(todo){
      todo.title = this.beforeEditCache
      todo.editing  = false
    },

    checkedAllTodo: function(){
      this.todos.forEach((todo) => todo.complete = event.target.checked)
    },

    clearCompleteTodos(){
      this.todos = this.todos.filter(todo => !todo.complete)
    },

    allTodos(){
      this.todos
    },

    activeTodos(){
      this.todos = this.todos.filter(todo => !todo.complete)
    },

    completeTodos(){
      this.todos = this.todos.filter(todo => todo.complete)
    },

  },

  computed: {
    remaining(){
      return this.todos.filter(todo => !todo.complete).length
    },

    anyRemaining(){
      return this.remaining != 0
    },

    todosFiltered(){
      // --
      return this.todos
      // --
    },

    plural(){
      return this.remaining <= 1 ? 'item' : 'items'
    },

  },




  // END
}


</script>

<style media="screen" scoped>

.container{
  width:60%;
  margin: 0 auto;
}

.todo_input{
  border-radius: 5px;
}

.completed{
  text-decoration: line-through;
  color: gray;
}

</style>
