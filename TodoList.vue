<template>
  <div>
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
      <template #list-item="{ data }">
        <div class="flex items-center">
          <div class="w-full flex justify-between items-center">
            <div :class="{ 'line-through text-slate-300': data.done }">
              <b>{{ data.text }}</b>
            </div>
            <div>
              <DxButton
                class="flex items-center justify-center dx-button-size"
                icon="edit"
                @click="editItem(data, $event)"
              />
            </div>
          </div>
        </div>
      </template>
    </DxList>
  </div>
</template>

<script>
import DxCheckBox from 'devextreme-vue/check-box'
import DxList from 'devextreme-vue/list'
import { DxButton } from 'devextreme-vue'

const listRefId = 'superSache'

export default {
  components: {
    DxCheckBox,
    DxList,
    DxButton
  },

  props: {
    items: {
      type: Array,
      required: true
    }
  },

  data() {
    return {
      listItems: this.items,
      listRefId,
      allowDeletion: true,
      itemDeleteMode: 'static',
      selectionMode: 'all'
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
