<template>
    <div v-if="remaining==1">{{ remaining }} item left</div>
    <div v-else>{{ remaining }} items left</div>
</template>

<script>
export default{
  name: 'todo-remaining',
  computed: {
    remaining(){
      return this.$store.getters.remaining
    }
  }
}

</script>
