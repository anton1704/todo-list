<template>
  <div id="app">
    <header class="bg-slate-400 text-white h-64">
      <div class="text-3xl text-center pt-10">Wilkommen auf ihrer persönlichen ToDo Liste!</div>
      <todo-text-input-vue @item-added="addItemToList"></todo-text-input-vue>
    </header>
    <todo-list ref="testVier" :items="listItems"> </todo-list>
  </div>
</template>

<script>
import TodoTextInputVue from './components/TodoTextInput.vue'
import TodoList from './components/TodoList.vue'

const listItems = [
  { text: 'Hello', done: false, icon: 'default', count: 12 },
  { text: 'Hallo', done: false, icon: 'default', count: 3 },
  { text: 'Hola', done: false, icon: 'default', count: 23 },
  { text: 'Bonjour', done: false, icon: 'default', count: 4 }
]

export default {
  name: 'App',
  components: {
    TodoTextInputVue,
    TodoList
  },
  data() {
    return {
      listItems
    }
  },
  methods: {
    addItemToList(value) {
      this.listItems.push(value)
      /*       this.listItems.push({text: value, done: false, icon: 'default'})
       */ console.log(this.listItems)
      this.$refs.testVier.reloadList()
    }
  }
}
</script>
