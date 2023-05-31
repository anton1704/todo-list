<template>
  <div id="app">
    <header class="bg-slate-400 text-white h-32">
      <div class="text-3xl text-center pt-10">Mach das!</div>
      <DxButton class="ml-2" @click="addNewList">Neue Liste</DxButton>
    </header>
    <TodoListContainer :lists="lists" @update-lists="updateLists" />
  </div>
</template>

<script>
import TodoListContainer from './components/TodoListContainer.vue'
import DxButton from 'devextreme-vue/button'

export default {
  name: 'App',
  components: {
    TodoListContainer,
    DxButton
  },
  data() {
    return {
      lists: []
    }
  },
  methods: {
    addNewList() {
      const newList = {
        name: 'Neue Liste',
        items: []
      }
      this.lists.push(newList)
    },
    updateLists(updatedLists) {
      this.lists = updatedLists
    }
  }
}
</script>
