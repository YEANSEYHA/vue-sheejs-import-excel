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
    
    <button  v-on:click="onSubmit()">
      Import
    </button>
  </div>
</template>

<script>
var XLSX = require('xlsx');
export default {
  data() {
    return {
      excelData: [],
      validateData: []
    };
  },
  methods: {
    excelExport(event) {
      var input = event.target;
      var reader = new FileReader();
      reader.onload = () => {
        var fileData = reader.result;
        var wb = XLSX.read(fileData, {type : 'binary'});
        console.log(wb)
        wb.SheetNames.forEach((sheetName) => {
            /* eslint-disable */
            var rowObj =XLSX.utils.sheet_to_json(wb.Sheets[sheetName]);
            this.excelData = JSON.stringify(rowObj)
            this.validateData = rowObj
            /* eslint-enable */
        })
      };
      reader.readAsBinaryString(input.files[0]);
    },
    onSubmit(){
      let data= [];
      data = this.validateData
      console.log(data[0].name_kh)
      console.log(data.length)

      /* if(((typeof data[0].name_kh != "undefined") && (typeof data[0].name_kh.valueOf() == "string")) && (data[0].name_kh.legnth > 0)){
        alert('This is a string')
      }else{
        alert('This is Not a string')
      } */

      let patternEmail = /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/; //eslint-disable-line
      //let patternName = /^[a-zA-Z\s]+$/;
      //let patternPhone = /^0[1-9]\d{7,8}$/;

      /* if((data[0].name_kh).match(patternName)){
        alert('Valid name')
      }else{
        alert('Invalid name')
      } */

      if((data[0].email).match(patternEmail)){
        console.log(data[0].email+'is valid.')

      }else{
        console.log(data[0].email+'Email is invalid')
      }

      /* if((data[0].phone).match(patternPhone)){
        console.log('Valid Phonenumber')
      }else{
        console.log('Invalid Phonenumber')
      } */
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