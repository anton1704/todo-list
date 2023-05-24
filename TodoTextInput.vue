<template>
  <div id="container">
    <DxTextBox
      class="text-black w-1/4 ml-[38%]"
      v-model:value="value"
      mode="url"
      label="Eingabe"
      label-mode="floating"
      :max-length="20"
      @enter-key="addItem"
    />
  </div>
</template>

<script>
import { DxTextBox } from 'devextreme-vue/text-box'

export default {
  components: {
    DxTextBox
  },
  data() {
    return {
      value: ''
    }
  },
  methods: {
    addItem() {
      this.listItems = []
      this.$emit('item-added', this.value)
      this.value = ''
    }
  }
}
</script>
