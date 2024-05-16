<template>
  <div>
    <input type="file" @change="handleFileUpload" />
    <div v-html="wordContent"></div>
  </div>
</template>

<script>
import mammoth from 'mammoth'
import * as util from 'mammoth/lib/util'

export default {
  data() {
    return {
      wordContent: '',
    }
  },
  methods: {
    handleFileUpload(event) {
      const file = event.target.files[0]
      const reader = new FileReader()

      reader.onload = async (e) => {
        const arrayBuffer = e.target.result
        try {
          const { value } = await mammoth.convertToHtml({ arrayBuffer })
          this.wordContent = value
        } catch (error) {
          console.error('Error converting document:', error)
        }
      }

      reader.readAsArrayBuffer(file)
    },
  },
}
</script>
