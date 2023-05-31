<template>
  <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
    <div v-for="(list, index) in lists" :key="index">
      <TodoList
        :items="list.items"
        :listName="list.name"
        @list-name-updated="updateListName(index, $event)"
      />
    </div>
  </div>
</template>

<script>
import TodoList from './TodoList.vue'

export default {
  components: {
    TodoList
  },
  props: {
    lists: {
      type: Array,
      required: true
    }
  },
  methods: {
    updateListName(index, newName) {
      this.lists[index].name = newName
    }
  }
}
</script>
