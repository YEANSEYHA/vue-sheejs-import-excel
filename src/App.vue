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
      
      // Validation Object
      const patterns ={
        name_latin:  /^[a-zA-Z\s]+$/,
        email: /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/, //eslint-disable-line
        phone: /[1-9]\d{7,8}$/,
        address: /^(?!\s*$).+/, // not empty string
        role: /^(?!\s*$).+/, // not empty string
        gender: /^(?!\s*$).+/, // not empty string
        birth_date:/^\d{1,2}\/\d{1,2}\/\d{4}$/
      }
      let countInvalid = 0;

      for (let i=0;i<data.length;i++){

        if((data[i].name_latin.toString()).match(patterns.name_latin)){
          console.log('Row No',i+1,'Name Latin:',data[i].name_latin,'is valid')
        }else{
          countInvalid++;
          console.log('Row No',i+1,'Name Latin:',data[i].name_latin,'is invalid')
        }
        
        if((data[i].phone.toString()).match(patterns.phone)){
          console.log('Row No',i+1,'Phone:',data[i].phone,'is valid')
        }else{
          countInvalid++;
          console.log('Row No',i+1,'Phone:',data[i].phone,'is invalid')
        }

        if((data[i].email.toString()).match(patterns.email)){
          console.log('Row No',i+1,'Email:',data[i].email,'is valid')
        }else{
          countInvalid++;
          console.log('Row No',i+1,'Email:',data[i].email,'is invalid')
        }

        if((data[i].address.toString()).match(patterns.address)){
          console.log('Row No',i+1,'Address:',data[i].address,'is valid')
        }else{
          countInvalid++;
          console.log('Row No',i+1,'Address:',data[i].address,'is invalid')
        }

        if((data[i].role.toString()).match(patterns.role)){
          console.log('Row No',i+1,'Role:',data[i].role,'is valid')
        }else{
          countInvalid++;
          console.log('Row No',i+1,'Role:',data[i].role,'is invalid')
        }

        if((data[i].gender.toString()).match(patterns.gender)){
          console.log('Row No',i+1,'Gender:',data[i].gender,'is valid')
        }else{
          countInvalid++;
          console.log('Row No',i+1,'Gender:',data[i].gender,'is invalid')
        }

        if((data[i].birth_date.toString()).match(patterns.birth_date)){
          console.log('Row No',i+1,'Birth Date:',data[i].birth_date,'is valid')
        }else{
          countInvalid++;
          console.log('Row No',i+1,'Birth Date:',data[i].birth_date,'is invalid')
        }


      }
      console.log('Total invalid field :',countInvalid)
      
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