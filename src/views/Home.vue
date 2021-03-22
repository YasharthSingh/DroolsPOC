<template>
  <div>
    

    <b-container class="bv-example-row">
  <b-row>
    <b-col lg="12">
        <b-img src="https://www.simplify3x.com/wp-content/uploads/2019/02/simp3x-2.png" fluid alt="Responsive image" style="margin-bottom: 10px; margin-top: 10px;"></b-img>
    </b-col>
    <b-col lg="12">
      <b-form-file
      v-model="file1"
      :state="Boolean(file1)"
      variant="primary"
      placeholder="Choose a file or drop it here..."
      drop-placeholder="Drop file here..."
    ></b-form-file>
    <button @click="onProcessClick">Uploaded</button>
    <div class="mt-3">Selected file: {{ file1 ? file1.name : '' }}</div>
    
    </b-col> 
    <b-col lg="12">
      <b-progress class="mt-2" :max="max" show-progress animated height="2rem">
      <b-progress-bar :value="value1" variant="success">Input Transcribed</b-progress-bar>
      <b-progress-bar :value="value2" variant="warning">Data Stream Validated</b-progress-bar>
      <b-progress-bar :value="value3" variant="primary">Rules Applied</b-progress-bar>
      <b-progress-bar :value="value4" variant="dark">Output Processed</b-progress-bar>
    </b-progress>
    </b-col>

    <b-col lg="12" style="margin-top: 20px;">
 <div>
    <b-table striped hover :items="items"></b-table>
  </div>

<div><h3>Response</h3>
<div v-for="(item, index) in apiresponsekeys" :key="index" style="height: 20px; border: 1px solid black;">
  {{ item }} &nbsp;&nbsp;&nbsp; | <b>{{ apiresponse[item] }}</b>
  </div>
  </div>
    </b-col>
  </b-row>
</b-container>


    
    </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "Home",
  
  data() {
    return {
      file1: null,
        value1: 25,
        value2: 25,
        value3: 25,
        value4: 25,
        max: 100,
        text: '',
        apiresponse : {},
        apiresponsekeys: [],
        
      baseURL : "http://127.0.0.1:8080/",

      items: [
          { age: 40, first_name: 'Dickerson', last_name: 'Macdonald' },
          { age: 21, first_name: 'Larsen', last_name: 'Shaw' },
          { age: 89, first_name: 'Geneva', last_name: 'Wilson' },
          { age: 38, first_name: 'Jami', last_name: 'Carney' }
        ]
    }
  },
  methods:{
    async onProcessClick(){
      var temptype = this.file1.name.split('.')[this.file1.name.split('.').length - 1];
      
      if(temptype == "tiff"){
        this.onUploadFnIMG();
      }
      if(temptype == "jpeg"){
        this.onUploadFnIMG();
      }
      if(temptype == "edi"){
        this.onUploadFnEDI();
      }
       
    },
    async onUploadFnIMG(){ 
      let data = new FormData();
      data.append("pdf", this.file1);
     
      await axios({
  method: "post",
  url: "http://167.71.228.147:5000/upload",
  data,
  headers: { "Content-Type": "multipart/form-data" },
})
  .then( (response) =>{
    //handle success
    console.log(response.data);
    this.apiresponsekeys = Object.keys(response.data);
    this.apiresponse = response.data;
  })
  .catch( (response)=> {
    //handle error
    
    console.log(response);
  });
},
    async onUploadFnEDI(){ 
      console.log('sad');
    }      
  }
};
</script>
