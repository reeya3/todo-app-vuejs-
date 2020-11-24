<template>
  <div>
    <input
      type="text"
      class="todo-input" 
      placeholder="Add todos from here"
      v-model="newTodo"
      @keyup.enter="addTodo()"
    >
    <transition-group
     enter-active-class="animated fadeInUp" 
     leave-active-class="animated rollOut"
    >
    <TodoItem 
      v-for="(todo, index) in todosFiltered"
      :key= "index"
      :todo= "todo"
      :index= "index"
      :checkAll= "!anyRemaining"
      @removedTodo= "removeTodo" 
      @finishedEdit= "finishedEdit"
    >
     
    </TodoItem>
  </transition-group>
    <div class="extra-container">
      <div><label><input type="checkbox" :checked="!anyRemaining" @click="checkAllOption">Check All</label></div>
      <div> {{ remaining }} items left</div>
    </div>
    <div class="extra-container">
      <div>
        <button 
          :class="{ active: filter == 'all' }" 
          @click="filter = 'all'"
        >
          All
        </button>
        <button 
          :class="{ active: filter == 'active' }" 
          @click="filter = 'active'"
          class="btn-second" 
        >
          Active
        </button>
        <button 
          :class="{ active: filter == 'completed' }"
          @click="filter = 'completed'"
        >
          Completed
        </button>
      </div>
    <div>
    </div>
      <transition>
        <button v-if="showCompletedBtn" @click="clearCompleted">clear completed</button>
      </transition>
    </div>
  </div>
</template>
<script>
  import TodoItem from './TodoItem'
export default {
  name: 'TodoList',
  components: {
    TodoItem
  },
  data() {
    return {
      newTodo: '',
      todoId: 3,
      beforeEditCache: '',
      filter: 'all',

      todos: [
      {
        id: 1,
        title: 'first work',
        completed: false,
        edit: false
      },
      {
        id: 2,
        title: 'second work',
        completed: false,
        edit: false

      }
      ]
    }
  },
 
    methods: {
      addTodo() {
        if(this.newTodo.trim().length == '') {
          return //doesnot let u to enter blank todo
        }
        this.todos.push({
          id: this.todoId,
          title: this.newTodo,
          completed: false,
          edit: false
        })
        this.newTodo= ''
        this.todoId++
      },
      cancleEdit(todo) {
        todo.title = this.beforeEditCache
        todo.edit = false
      },
       removeTodo(index) {
        this.todos.splice(index, 1)
      },
      checkAllOption() {
        this.todos.forEach((todo) => todo.completed = event.target.checked)
      },
      clearCompleted() {
        this.todos = this.todos.filter(todo => !todo.completed )
      },
      finishedEdit(data) {
        this.todos.splice(data.index, 1 ,data.todo)
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
      }
      else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
      } 
      else if (this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    },
     showCompletedBtn() {
        return this.todos.filter(todo => todo.completed).length > 0
    }
   }
  }
  
  
</script>
<style>
  @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css"); 
  .todo-input {
    width: 100%;
    padding: 0.5rem 1rem;
    margin-bottom: 2rem;
    font-size: 1.11rem;
  }
  .todo-item {
    display: flex;
    margin-bottom: 1rem;
    justify-content: space-between;
    animation-duration: 0.4s;
  }
  .remove-items {
    cursor: pointer;
    margin-bottom: 1rem;
    &:hover {
      color: black;
    }
  }
  .todo-item-left {
    display: flex;
    align-items: center;
  }
 .todo-item-label {
    padding: 10px;    
    border: 1px solid white;
    margin-left: 1rem;
 }
 .todo-item-edit {
    padding: 7px;    
    border: 1px solid gray;
    margin-left: 1rem;
    font-size: 1.17rem;
    width: 100%

 }
 .completed {
  text-decoration: line-through;
  color: gray;
 }
 .extra-container {
    display: flex;
    justify-content: space-between;
    border-top: 1px solid blue;
    margin-bottom: 10px;
    padding-top: 10px;
 }
 .active {
  background-color: lightblue;
 }
 .btn-second {
  margin-right: 1rem;
  margin-left: 1rem;

 } 
 .fade-enter-active, .fade-leave-active {
    transition: opacity .2s;
  }
  
</style>