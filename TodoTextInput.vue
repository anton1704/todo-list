<template>
  <div id="container">
    <DxTextBox
      class="text-black w-1/4 ml-[38%]"
      v-model:value="value"
      mode="url"
      label="Eingabe"
      label-mode="floating"
      @enter-key="handleEnterKey"
      @input="handleInput"
    />
  </div>
</template>

<script>
import { DxTextBox } from 'devextreme-vue/text-box'
import Swal from 'sweetalert2'

export default {
  components: {
    DxTextBox
  },
  data() {
    return {
      value: ''
    }
  },
  computed: {
    isInputValid() {
      const regex = /^[a-zA-Z0-9]+$/
      const minLength = 3
      const maxLength = 20
      return (
        this.value.length >= minLength &&
        this.value.length <= maxLength &&
        regex.test(this.value.trim())
      )
    }
  },

  methods: {
    addItem() {
      if (this.isInputValid) {
        this.listItems = []
        this.$emit('item-added', this.value)
        this.value = ''
      }
    },
    handleEnterKey() {
      if (!this.isInputValid) {
        Swal.fire({
          title: 'Vorsicht',
          text: 'Der eingegebene Text ist nicht zulässig!',
          icon: 'error',
          confirmButtonText: 'OK'
        })
      } else {
        this.addItem()
      }
    }
  }
}
</script>
