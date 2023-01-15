<template>
    <div class="todo-item">
      <div class="todo-item-left">
        <input type="checkbox" v-model="completed" @change="doneEdit">
        <!-- j'affiche l'edition si je double click sur la tâche-->
        <div v-if="!editing" @dblclick="editTodo" class="todo-item-label"
        :class="{ completed: completed }">{{ name }}</div>
        <input v-else class="todo-item-edit" type="text" v-model="name" @blur="doneEdit"
         @keyup.enter="doneEdit" @keyup.esc="cancelEdit">
      </div>
      <div>
        <span class="remove-item" @click="removeTodo(index)">
          &times;
        </span>
      </div>
    </div>
</template>


<script>
export default{
  name: 'todo-item',
  // props: ['todo']

  props:{
    todo: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
      required: true
    },
    checkAll:{
      type: Boolean,
      required: true
    }
  },

  data() {
    return{
      'id': this.todo.id,
      'name': this.todo.name,
      'completed': this.todo.completed,
      'editing': this.todo.editing,
      'beforeEditCache': ''
    }
  },

  // created(){
  //   eventBus.$on('pluralize', this.handlePluralize)
  // },

  // beforeDestroy(){
  //   eventBus.$off('pluralize', this.handlePluralize)
  // },

  watch: {
    checkAll(){
      // if(this.checkAll){
      //   this.completed = true
      // }else{
      //   this.completed = this.todo.completed
      // }
      this.completed =this.checkAll ? true : this.todo.completed
    }
  },

  methods: {
    //Supprimer une tâche
    removeTodo(id){
      this.$store.dispatch('deleteTodo', id)
    },

    //fonction double click pour faire apparaître le input pour editer
    editTodo(){
      this.beforeEditCache = this.name
      this.editing = true
    },

     //Lorsque le input edit est vide et qu'on valide , le text par defaut s'affiche
     doneEdit(){
      if(this.name.trim() == ''){
        this.name = this.beforeEditCache
      }
      this.editing = false
      this.$store.dispatch('updateTodo', {
        'id':this.id,
        'name': this.name,
        'completed': this.completed,
        'editing': this.editing
      })
    },

    cancelEdit(){
      this.name = this.beforeEditCache
      this.editing = false
    },

    // handlePluralize(){
    //   this.name = this.name +'s'
    //   const index = this.$store.state.todos.findIndex(item => item.id == this.id)
    //   this.$store.state.todos.splice(index, 1, {
    //     'id':this.id,
    //     'name': this.name,
    //     'completed': this.completed,
    //     'editing': this.editing
    //   })
    // }
  }

}

</script>
