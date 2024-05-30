<template>
      <div id="wrapper">
          <h1 id="header">Weather App</h1>
          <div id="inputs">
            <input type="text" id="inputCity" placeholder="Enter a city..." v-model="city">
            <button id="search" @click="searchWeather()">Search</button>
        </div>
        </div>
        <weather-comp :city="city" v-if="showWeather"/>
        <router-view/>
</template>

<script>
import weatherComp from './components/weatherComp.vue'
export default {
  data(){
    return{
      city:"",
      showWeather: false,
    }
  },
  components:{
    weatherComp,
  },  
  methods:{
      async searchWeather(){
      this.showWeather = false;
      await this.$nextTick();
      this.showWeather = true;
    }
  },
}
</script>

<style>
body{
  background-color:rgba(17, 17, 19, 1);
  font-family: "Open Sans", sans-serif;
}
#header{
  color:white;
  top:50px;
  left:350px;
  font-size:40px;
  margin-bottom:10px;
  display:flex;
  justify-content: center;
}
#wrapper{
  margin-left:auto;
  margin-right:auto;
  width:1400px;
  height:27vh;
  margin-top:100px;
  position:relative;
  background: #212730;
  border-radius:10px; 
}
#inputCity{
  height:50px;
  width:650px;
  border-radius:10px;
  background:white;
  outline: none;
  margin-left:150px;
  margin-top:30px;
}
#inputCity:focus{
  border-bottom:1px solid blue;
}
#search{
  margin-top:25px;
  margin-left:20px;
  width:80px;
  height:43px;
  background:rgba(111, 198, 248, 0.8);
  border-radius:10px;
  color:white;
}
#search:hover{
  background: rgba(39, 54, 245, 0.8);
}
#inputs{
  width:100%;
  display:flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}


</style>
