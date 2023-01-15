<template>
  <div>
    <!-- Ajouter une tâche -->
    <input type="text" class="todo-input" placeholder="What needs to be done?" v-model="newTodo"
     @keyup.enter="addTodo">
    <todo-item  v-for="(todo,index) in todosFiltered" :key="todo.id" :todo="todo" :index="index"
      :checkAll="!anyRemaining">
    </todo-item>

    <div class="extra-container">
      <!-- component pour selectionner toutes les tâches, et aussi une par une qui automatiquement
      affecte le checkAll-->
      <todo-check-all></todo-check-all>
      <!--component incrémente à chaque fois qu'une tâche est ajoutée-->
      <todo-items-remaining></todo-items-remaining>

    </div>

    <div class="extra-container">
      <todo-filtered></todo-filtered>

      <div>
        <transition name="fade">
          <todo-clear-completed></todo-clear-completed>
        </transition>
      </div>
    </div>

  </div>
</template>

<script>
import TodoItem from './TodoItem.vue'
import TodoItemsRemaining from './TodoItemsRemaining.vue'
import TodoCheckAll from './TodoCheckAll.vue'
import TodoFiltered from './TodoFiltered.vue'
import TodoClearCompleted from './TodoClearCompleted.vue'

export default {
  name: 'todo-list',
  components: {
    TodoItem,
    TodoItemsRemaining,
    TodoCheckAll,
    TodoFiltered,
    TodoClearCompleted
  },
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
    }
  },
  created(){
    this.$store.dispatch('retrieveTodos')
  },
  computed: {
    //si je sélectionne toutes les tâches un par un, automatiquement le check all est impacté
    anyRemaining(){
      return this.$store.getters.anyRemaining
    },
    //filtrer les tâches par all, active, completed
    todosFiltered(){
      return this.$store.getters.todosFiltered
    }
  },
  methods: {
    //return rien si c'est un espace envoyé
    addTodo(){
      if(this.newTodo.trim().length == 0){
        return
      }

      this.$store.dispatch('addTodo',{
        id: this.idForTodo,
        name: this.newTodo,
      })

      this.newTodo = '',
      this.idForTodo++
    },
  }
}
</script>


<style >
/* Centrer le input et rendre un peu grand */
  .todo-input{
    width: 100%;
    padding: 10px 18px;
    font-size: 18px;
    margin-bottom: 16px;
  }

  /*Espace entre chaque tâche ajoutée*/
  .todo-item{
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  /*supprimer tache*/
  .remove-item{
    cursor: pointer;
    margin-left: 14px;
    align-items: center;
    justify-content: space-between;
  }

  .todo-item-left{
    display: flex;
    align-items: center;
  }

  .todo-item-edit{
    font-size: 24px;
    color:#2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    font-family: 'Avenir', Arial, Helvetica, sans-serif;
  }

  /* rayé la tâche .*/
  .completed{
    text-decoration: line-through;
    color: gray;
  }

  .extra-container{
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 14px;
    margin-bottom: 14px;
  }

  button{
    font-size: 14px;
    background-color: white;
    appearance: none;
  }

  .active{
    background: lightgreen;
  }

</style>
