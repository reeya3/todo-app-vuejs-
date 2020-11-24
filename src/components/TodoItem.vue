<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input 
        type="checkbox" 
        v-model="completed"
        @change="doneEdit"
      >
      <div
        class="todo-item-label"
        v-if="!edit"
        @dblclick="editTodo"
        :class="{completed:completed}"
      >
        {{ todo.title }}
      </div>
      <div v-else>
        <input 
          class="todo-item-edit"
          type="text" 
          v-model="title"
          @blur="doneEdit"
          @keyup.enter="doneEdit" v-focus
          @keyup.esc= "cancleEdit"
        >
      </div>
    </div>
    <div class="remove-items" @click="removeTodo(index)">
      &times;
    </div>
    
  </div>
</template>
<script>
  export default {
    props: {
      todo: {
        type: Object,
        required: true
      },
      index: {
        type: Number,
        required: true
      },
      checkAll: {
        type: Boolean,
        required: true
      }
    },
    watch: {
      checkAll() {
       if (this.checkAll) {
        this.completed = true
      } else {
        this.completed = this.todo.completed
      }
       /* this.completed = this.checkAll ? true : this.todo.completed
      */
      }
    },
    data() {
      return {
        id: this.todo.id,
        title: this.todo.title,
        completed: this.todo.completed,
        edit: this.todo.edit,
        beforeEditCache: ''
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
      removeTodo(index) {
        this.$emit('removedTodo', index)
      },
      editTodo() {
        this.beforeEditCache = this.title
        this.edit = true
      },
      doneEdit() {
         if (this.title.trim() == '') {
          this.title = this.beforeEditCache
      }
        this.edit = false
        this.$emit('finishedEdit', {
          index: this.index,
          todo: {
            id: this.id,
            title: this.title,
            completed: this.completed,
            edit: this.edit
          }
        })
      
      },
      cancleEdit(todo) {
        todo.title = this.beforeEditCache
        todo.edit = false
      },
    }
  }
</script>