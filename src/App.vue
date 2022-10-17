<template>
  <div id="app" :class="typeof forcastInfo.main!=='undefined' && forcastInfo.weather[0].main ==='Rain' ? 'rain' 
    : typeof forcastInfo.main!=='undefined' && forcastInfo.weather[0].main ==='Clouds'?'cloud'
    :typeof forcastInfo.main!=='undefined' && forcastInfo.weather[0].main ==='Haze'?'haze'
    :''">
    <main>
      <div class="form">
        <input type="text" v-model="city" Placeholder="Enter location here..." />
        <button @click="findCity"><img src="@/assets/search.png"></button>
      </div>
      <div class="error" v-if="hasError">
          <span>Enter a valid city </span>
        </div>
      <div class="weather-container" v-if="showForcast">
        <div class="location">
          <p>{{forcastInfo.name}} , {{forcastInfo.sys.country}}</p>
        </div>
        <div class="date">{{getDate()}}</div>
        <div class="temp">
          <p>{{Math.round(forcastInfo.main.temp)}}&deg;C </p>
        </div>
    
        <div class="condition">
          <p>{{forcastInfo.weather[0].main}}</p>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "App",
  data(){
    return{
      city:'',
      forcastInfo:{},
      api_key:'88a27c055097366c3e000a94df9ec2b3',
      // url:'https://api.openweathermap.org/data/2.5/weather?q={city name}&appid={API key}',
      showForcast:false,
      hasError:false
    }

  },
  methods:{
    findCity(){
      if(!this.city){
        this.hasError = true
      }else{
        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${this.api_key}&units=metric`)
        .then(res=>{
          this.showForcast = true
          this.forcastInfo = res.data
          console.log(this.forcastInfo)
          this.hasError = false
        })
      }
    },
    getDate(){
      const date = new Date();
      let day = date.getDate();
      let month = date.getMonth() + 1;
      let year = date.getFullYear();
      return `${day}-${month}-${year}`;
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-image: url(@/assets/Sunny.jpg); 
  /* background-image: url(@/assets/Warm.jpg);  */
  /* background-image: url(@/assets/cloudy.jpg); */
  background-size: cover;
  background-position: top;
  background-repeat: no-repeat;
  transition: all .3s; 
}
#app .cloud{
     background-image: url(@/assets/cloud1.jpg);
}
#app .rain{
  background-image: url(@/assets/rain-3.gif);
}
#app .haze{
  background-image: url(@/assets/haze.gif);
}
main {
  width: 100%;
  height: 100vh;
  background: none;
  background-color: rgba(31, 30, 30, 0.75);
  display: flex;
  align-items: center;
  flex-direction: column;
}
.form{
  display:flex;
  align-items: center;
  justify-content: center;
}
input[type="text"] {
  margin-top: 50px;
  padding: 10px 15px;
  width:450px;
  outline: none;
  border-radius: 10px;
  border: none;
  box-shadow: 3px 3px 3px 3px rgba(109, 109, 109, 0.25) ;
}

button {
  margin-left: 30px;
  margin-top: 50px;
  padding:5px 10px;
  /* width: 10px; */
  outline: none;
  border-radius: 7px;
  color: white;
  background: rgba(77, 76, 76, 0.4);
  cursor: pointer;
  border: none;
}
button > img{
 width:30px;
 height: 30px;
}
.error{
  text-align: center;
  margin:20px 0;
  transition:all 1s;
}
.error > span{
  background-color: #fff;
  color:red;
  padding:10px 80px;
  border-radius:7px;
}
.location{
  margin-top: 40px;
  color: white;
  text-align: center;
  font-size: 2em;
}
.date{
  color:white;
  margin-top:5px;
  text-align: center;
}
.temp{
  margin:20px;
  color:rgb(243, 236, 236);
  font-size:4rem;
  font-weight: 900;
  text-align:center;
  padding:30px 20px;
  box-sizing: border-box;
  text-shadow: 3px 5px rgba(110, 109, 109, 0.75);
  border-radius: 5px;
  box-shadow: 3px 3px 3px 3px rgba(56, 54, 54, 0.75) ;
  background: rgba(77, 76, 76, 0.25);
}
.condition{
  margin:40px;
  font-size:2em;
  text-align: center;
  font-weight: 700;
  color: white;
}
@media Screen and (max-width:768px){
  input[type="text"]{
    width:350px;
  }
}
@media Screen and (max-width:540px){
  input[type="text"]{
    width:300px;
  }
}
@media Screen and (max-width:480px){
  input[type="text"]{
    width:220px;
    padding:10px;
  }
  button{
    margin-left:20px;
    padding:10px;
    width:70px;
  }
}
</style>
