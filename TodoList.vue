<template>
  <div>
    <div class="flex justify-between items-center mb-4">
      <h2 class="text-2xl font-semibold">{{ listName }}</h2>
      <DxButton class="dx-button-size" icon="edit" @click="editListName" />
    </div>
    <DxList
      :data-source="listItems"
      :ref="listRefId"
      item-template="list-item"
      :allow-item-deleting="allowDeletion"
      :item-delete-mode="itemDeleteMode"
      :show-selection-controls="true"
      :selection-mode="selectionMode"
      @selectionChanged="onSelectionChange"
    >
      <template #list-item="{ data, index }">
        <div class="flex justify-between items-center">
          <div :class="{ 'line-through text-slate-300': data.done }">
            <b>{{ data.text }}</b>
            <br />
            <span v-if="data.date">Zu erledigen bis: {{ formatDate(data.date) }}</span>
          </div>
          <div>
            <DxButton
              class="flex items-center justify-center dx-button-size"
              icon="edit"
              @click="editItem(data, $event)"
            />
            <DxButton
              class="flex items-center justify-center dx-button-size"
              icon="event"
              @click="toggleCalendar(index, $event)"
            />
            <DxCalendar
              v-if="showCalendarIndex === index"
              :value="data.date"
              @valueChanged="onDateChange(data, $event)"
            />
          </div>
        </div>
      </template>
    </DxList>
  </div>
</template>

<script>
import DxCheckBox from 'devextreme-vue/check-box'
import DxList from 'devextreme-vue/list'
import DxButton from 'devextreme-vue/button'
import DxCalendar from 'devextreme-vue/calendar'

const listRefId = 'superSache'

export default {
  components: {
    DxCheckBox,
    DxList,
    DxButton,
    DxCalendar
  },

  props: {
    items: {
      type: Array,
      required: true
    },

    listName: {
      type: String,
      required: true
    }
  },

  data() {
    return {
      listItems: this.items,
      listRefId,
      allowDeletion: true,
      itemDeleteMode: 'static',
      selectionMode: 'all',
      showCalendarIndex: -1
    }
  },

  methods: {
    reloadList() {
      this.list.reload()
      this.list.repaint()
    },

    onSelectionChange(value) {
      value.addedItems.forEach((item) => {
        const selectedItem = this.listItems.find((listItem) => listItem.text === item.text)
        if (selectedItem) {
          selectedItem.done = true
        }
      })

      value.removedItems.forEach((item) => {
        const removedItem = this.listItems.find((listItem) => listItem.text === item.text)
        if (removedItem) {
          removedItem.done = false
        }
      })
    },

    editItem(item, e) {
      const newText = prompt('Enter new text:', item.text)
      if (newText) {
        item.text = newText
      }
      e.event.stopPropagation()
    },

    toggleCalendar(index, e) {
      if (this.showCalendarIndex === index) {
        this.showCalendarIndex = -1
      } else {
        this.showCalendarIndex = index
      }
      e.event.stopPropagation()
    },

    onDateChange(item, event) {
      item.date = event.value
      this.showCalendarIndex = -1
      event.event.stopPropagation()
    },

    formatDate(date, e) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString(undefined, options)
    },

    editListName() {
      const newName = prompt('Enter new name:', this.listName)
      if (newName) {
        this.$emit('list-name-updated', newName)
      }
    }
  },

  computed: {
    list() {
      return this.$refs[listRefId].instance
    }
  }
}
</script>

<style>
.dx-button-size {
  width: 26px;
  height: 26px;
}
</style>
