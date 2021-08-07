<template>
  <div id="app" :class="typeof weather.main  != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <h1>OB-HAVO</h1>
    <main>
      <div class="search__box">
        <input 
        type="text" 
        class="search-bar" 
        placeholder="Search..." 
        v-model="query"
        @keypress="fetchWeather"
        >
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>
<script>
export default{
  name:'app',
  data(){
    return{
      api__key: 'aa98a548a5bee59e3de9961ccc7d1602',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query:'',
      weather:{}
    }
  },
  methods:{
    fetchWeather(e){
      if (e.key == 'Enter') {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api__key}`)
        .then(response =>{
          return response.json()
        }).then(this.setResults)
      }
    },
    setResults(results){
      this.weather = results
    },
    dateBuilder(){
      let d = new Date()
      let months = ["January","February","March","April","May","June","July","August","September","October","November","December"]
      let week = ['Monday','Tuesday','Wednesday','Thursday','Friday','Saturday','Sunday']

      let day = week[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let years = d.getFullYear()

      return `${day} ${date} ${month} ${years}`
    }
  }
}
</script>
<style lang="scss">
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: 'montserrat', sans-serif;
}
#app{
  background-image: url(./assets/COULD.jpg);
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
  transition: .4s;
  position: relative;
  h1{
display: inline-block;
position: absolute;
top: 25px;
left: 0;
right: 0;
margin: auto;
    padding: 10px 25px;
    color: #ffff;
    font-size: 40px;
    font-weight: 900;
    text-shadow: 3px 6px rgbA(0 0 0 / 25%);
    border-radius: 16px;
  }
}
main{
  display: grid;
  grid-template-columns:50% 50% ;
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,.25), rgba(0,0,0,8));
  .search__box{
    width: 90%;
    .search-bar{
      margin-top: 35vh;
      display: block;
      width: 100%;
      padding: 15px;
      color: rgba(11, 12, 11, 0.623);
      font-size: 20px;
      border:none;
      background: none;
      outline: none;
      appearance: none;
      box-shadow: 0px 0px 8px rgba(0,0,0,.26);
      background-color: rgba(117, 116, 116, 0.5);
      border-radius: 0px 16px 0px 16px;
      transition: .4s ;
      &:focus{
        border-radius: 16px 0px 16px 0px;
        background-color: rgba(255,255,255, .5);
        box-shadow: 0px 0px 16px rgba(0,0,0,.26);
      }
      }
  }
}
.location-box{
  .location{
    margin-top: 10vh;
    color: #fff;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0,0,0,.25);
  }
  .date{
    color: #fff;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
  }
}
.weather-box{
  text-align: center;
  .temp{
    display: inline-block;
    padding: 10px 25px;
    color: #ffff;
    font-size: 100px;
    font-weight: 900;
    text-shadow:3px 6px rgba(0,0,0,.25) ;
    background-color: rgba(255,255,255, .25);
    border-radius: 16px;
    margin: 30px 0;
    box-shadow: 3pxz 6px rgba(0,0,0,.25);
  }
  .weather{
    color: #fff;
    font-size: 28px;
    font-style: italic;
    text-shadow: 3px 6px rgba(0,0,0,.25);
  }
}
#app.warm{
  background-image: url('./assets/HOT.jpg');
}
</style>
