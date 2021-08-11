<!-- Use preprocessors via the lang attribute! e.g. <template lang="pug"> -->
<template>
  <div id="app">
    <h1>excel import with vue & sheet.js!</h1>    
    <div>
        <input type="file" id="excelFile" @change="excelExport" accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"/>
    </div>
    <div>
      {{ this.excelData }}
    </div>
  </div>
</template>

<script>
var XLSX = require('xlsx');
export default {
  data() {
    return {
      excelData: []
    };
  },
  methods: {
    excelExport(event) {
      var input = event.target;
      var reader = new FileReader();
      reader.onload = () => {
        var fileData = reader.result;
        var wb = XLSX.read(fileData, {type : 'binary'});
        wb.SheetNames.forEach((sheetName) => {
            /* eslint-disable */
            var rowObj =XLSX.utils.sheet_to_json(wb.Sheets[sheetName]);
            this.excelData = JSON.stringify(rowObj)
            console.log(rowObj)
            /* eslint-enable */
        })
      };
      reader.readAsBinaryString(input.files[0]);
    }
  }
};
</script>

<!-- Use preprocessors via the lang attribute! e.g. <style lang="scss"> -->
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>