<template>
  <div id="app">
    <header class="bg-slate-400 text-white h-64">
      <div class="text-3xl text-center pt-10">Mach das!</div>
      <todo-text-input-vue @item-added="addItemToList"></todo-text-input-vue>
    </header>
    <todo-list
      ref="testVier"
      :items="listItems"
      :list-name="listName"
      @list-name-updated="updateListName"
    />
  </div>
</template>

<script>
import TodoTextInputVue from './components/TodoTextInput.vue'
import TodoList from './components/TodoList.vue'

export default {
  name: 'App',
  components: {
    TodoTextInputVue,
    TodoList
  },
  data() {
    return {
      listItems: [],
      listName: 'My List'
    }
  },
  methods: {
    addItemToList(value) {
      this.listItems.push({ text: value, done: false, icon: 'default' })
      console.log(this.listItems)
      this.$refs.testVier.reloadList()
    },
    updateListName(newName) {
      this.listName = newName
    }
  }
}
</script>
