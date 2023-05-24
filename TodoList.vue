<template>
   
  <div id="list-api-demo">
       
    <div class="w-1/2 content-center">
           
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
          <div :class="{ 'line-through	': data.done }">
            <b>{{ data.text }}</b>
            <br />
            <p style="margin: 1px">{{ data.count }}</p>
          </div>
        </template>
      </DxList>
         
    </div>
  </div>
</template>

<script>
import DxSelectBox from 'devextreme-vue/select-box'
import DxCheckBox from 'devextreme-vue/check-box'
import DxList from 'devextreme-vue/list'

const listRefId = 'superSache'

export default {
  components: {
    DxSelectBox,
    DxCheckBox,
    DxList
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
    reloadList: function () {
      this.list.reload()

      this.list.repaint()
    },

    onSelectionChange: function (value) {
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
    }
  },

  computed: {
    list: function () {
      return this.$refs[listRefId].instance
    }
  }
}
</script>
