
<template>
  <body style = "margin-left: 10px !important;">
    <div class="container">
    <section class="graph-two">

    
    <h2>Video Platform Roadshow Stats</h2>
  <div class="bar-graph-container">
    <div class="bar-container">
     <div class="html" v-bind:style="{ width: update3 + '%' }">
       <h1>😁 {{ update3 }}</h1>
      </div>
    </div>
    
   <div class="bar-container">
     <div class="css" v-bind:style="{ width: update2 + '%' }">
      <h1>😶 {{ update2 }}</h1>
      </div>
    </div>
    
  	<div class="bar-container">
     <div class="js" v-bind:style="{ width: update1 + '%' }">
      <h1>🙁 {{ update1 }}</h1>
      </div>
    </div>
     

   </div>
  </section>
</div>
</body>
<footer>
  <div  v-if="dataa" id = "page">
    <!-- <h1>Hello 1!</h1> -->
    <!-- <h1>{{ status }}</h1> -->
    <video id = "vid" class="fadeinout" width="500" height="500" autoplay >
  <source src="./assets/SAD.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>
    
  </div>
  <div  v-if="data2" id = "page">
    <!-- <h1>Hello 2!</h1> -->
    <!-- <h1>{{ status }}</h1> -->
    <video id = "vid" class="fadeinout" width="500" height="500" autoplay >
  <source src="./assets/neutraL.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>
    
  </div>
  <div  v-if="data3" id = "page">
    <!-- <h1>Hello 3!</h1> -->
    <!-- <h1>{{ status }}</h1> -->
    <video id = "vid" class="fadeinout" width="500" height="500" autoplay >
  <source src="./assets/smile.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>
    
  </div>

</footer>
</template>


 <script>
//  import {ref} from 'vue';
 import axios from 'axios';
 import VueSocketIO from 'vue-socket.io';
 import io from 'socket.io-client';
//  const data = []
//  let status = ref(0)

 export default{
  data(){
    return {
      dataa:false,
      data2:false,
      data3:false,
      update1: localStorage.getItem("update1"),
      update2: localStorage.getItem("update2"),
      update3: localStorage.getItem("update3"),
    };
  },
  sockets:{
    insert(data){
      console.log("new data came in!")
      
      
    },
  },
  created(){
const socket = io('http://127.0.0.1:8080/');
console.log("new data came in!!!")

socket.on('connect', function () {
      console.log('connected to webSocket');
      //sending to server
      socket.emit('insert', { data: 'Connected from client!' });
    });
    socket.on('insert', (data) => {
      //this.getAllLayouts();
      console.log("data=",data);
      console.log("data message =",data.message);
      if (data.message == "1"){
        console.log("inside data message 1")
        this.dataa = true;
        console.log(this.dataa)
      setTimeout(() => {
        this.dataa = false;
      }, 5000);
      } else if(data.message == "2"){
        this.data2=true;
        setTimeout(() => {
        this.data2 = false;
      }, 5000);
      } else if(data.message == "3"){
        this.data3=true;
        setTimeout(() => {
        this.data3 = false;
      }, 5000);
      } else{
        console.log("Else Block Socket Message is:", data.message)
      }
      
    });

    socket.on('update', (data) => {
      console.log("Update ")
      if(data.response1){
        console.log("inside update 1: ", data.response1)
          this.update1 = data.response1
          localStorage.setItem("update1", this.update1);
      } else if (data.response2){
        this.update2 = data.response2
        localStorage.setItem("update2", this.update2);
      } else if(data.response3){
        this.update3 = data.response3
        localStorage.setItem("update3", this.update3);
      }
    });
  },
 };

</script>
