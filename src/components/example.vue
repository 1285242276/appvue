<template>
  <div>
    <input type="file" @change="handleFileUpload" />
    <table>
      <tr v-for="(row, rowIndex) in excelData" :key="rowIndex">
        <td v-for="(cell, cellIndex) in row" :key="cellIndex">{{ cell }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
import * as XLSX from 'xlsx'

export default {
  data() {
    return {
      excelData: [],
    }
  },
  methods: {
    handleFileUpload(event) {
      const file = event.target.files[0]
      const reader = new FileReader()

      reader.onload = (e) => {
        const data = new Uint8Array(e.target.result)
        const workbook = XLSX.read(data, { type: 'array' })

        const sheetName = workbook.SheetNames[0]
        const worksheet = workbook.Sheets[sheetName]
        const json = XLSX.utils.sheet_to_json(worksheet, { header: 1 })
        this.excelData = json
      }

      reader.readAsArrayBuffer(file)
    },
  },
}
</script>
