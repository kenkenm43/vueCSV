<template>
  <div style="text-align:center;" >
    <h1>Export JSON to Excel</h1>
    <button @click="getReport" >Export</button> <!-- เพิ่มปุ่ม Export -->
  </div>
</template>

<script>
export default {
  name: 'app',
  data(){
    return {
      // ข้อมูล JSON ทีต้องการ Export 5555
      json : [
        { name: 'Dady', age: '21' },
        { name: 'Jonh', age: '25' },
        { name: 'James', age: '17' },
      ]
    }
  },
  methods: {
    // เมื่อกดปุ่มจะทำการสร้างไฟล์ xcel ด้วย xlsx
    objectToCsv(data) {
      const csvRows = []
      const headers = Object.keys(data[0])
      csvRows.push(headers.join(','))

      for(const row of data) {
        const values = headers.map(header => {
          const escaped = (''+row[header]).replace(/"/g, '\\"')
          return `"${escaped}"`
        })
        csvRows.push(values.join(','))
      }
      return csvRows.join('\n')
    },
    download (data) {
      const blob = new Blob([data],{type:'text/csv'})
      const url = window.URL.createObjectURL(blob)
      const a = document.createElement('a')
      a.setAttribute('hidden','')
      a.setAttribute('href',url)
      a.setAttribute('download','download.csv')
      document.body.appendChild(a)
      a.click()
      document.body.removeChild(a)
    },
    async getReport() {
      const csvData = this.objectToCsv(this.json)
      this.download(csvData)
    }
  }
}
</script>
