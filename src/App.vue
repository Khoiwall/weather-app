<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp <= 16 ? 'warm' : ''">
    <main>
      <div class = "search-box">
        <input type = "text"
         name ="search" 
         class = "search-bar" 
         placeholder = "search...." 
         v-model="query"
         @keypress="fetchWeather"
         />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined' ">
        <div class="location-box">
          <div class ="location">{{ weather.name }}, {{weather.sys.country}}</div><br>
          <div class="date">{{ calendar() }}</div>
        </div>

        <div class ="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div><br>
          <div class="weather">{{ weather.weather[0].main }}</div>
      </div>
      </div>

    </main>
  </div>
</template>

<script>
  export default{
    name: 'app',
    data(){
      return{
        api_key: '7f4da88722dde7f8c871dbd866be4f50',
        url_base: 'https://api.openweathermap.org/data/2.5/',
        query: '',
        weather: {}
      }
    },
    methods: {
        fetchWeather(e){
          if (e.key == "Enter"){
            fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
              .then(res => {
                return res.json();
              }).then(this.setResults);
          }
        },
        setResults(results){
          this.weather = results;
        },
        calendar(){
          var d = new Date();
          var months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
          var days = ["Moday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"];

          var day = days[d.getDay()];
          var date = d.getDate();
          var month = months[d.getMonth()];
          var year = d.getFullYear();

          return `${day} ${date} ${month} ${year}`
        }
    }
  }
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
}

#app{
  background-image: url('./views/sun.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main{
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom,rgba(0,0,0, 0.25), rgba(0,0,0,0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  box-shadow: 0px 0px 0px rgba(0,0,0,0.25);
  background: none;
  background-color: rgba(255,255,255,0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus{
  box-shadow: 0px 0px 0px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box {
  text-align: center;
}

.location-box .location{
  font-size: 40px;
  font-weight: 500;
  color: #FFF;
  font-style: italic;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
  display: inline-block;
}

.location-box .date{
  font-size: 20px;
  font-weight: 300;
  color: #FFF;
  font-style: italic;
  display: inline-block;
}

.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0;
}

.weather-box .weather{
  color: #FFf;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  display:inline-block;
}

#app.warm{
  background-image: url('./views/cold.jpg');
}
</style>
