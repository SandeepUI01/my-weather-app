<template>
  <div id="app" >
    <div class="container" :class="typeof weather.main != 'undefined' && weather.main.temp > 20 ? 'warm' : 'cold'">
      <h1>Weather App</h1>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 

          placeholder="City Name"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap"  v-if="typeof weather.main != 'undefined'" >
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather-type">{{ weather.weather[0].main }}</div>
        </div>
      </div>
      
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: '2347febd2b043bb03a952020917b0256',
      url_base: 'https://api.openweathermap.org/data/2.5/',
       cnt :"&cnt=4",
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>
<style lang="scss" scoped>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
body {
	font-family: 'montserrat', sans-serif;
}
.container {
	min-height: 100vh;
	padding: 25px;
	width: 400px;
	margin: auto;
	background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
  &.warm{
    background-color: rgb(201, 150, 84);
  }
  &.cold{
    background-color: rgb(59, 144, 170);
  }
  h1{
    color:#FFF;
    margin: 10px auto 20px;
  }
}
.search-box {
	width: 100%;
	margin-bottom: 30px;
	.search-bar {
		display: block;
		width: 100%;
		padding: 15px;
		color: #313131;
		font-size: 20px;
		appearance: none;
		border: none;
		outline: none;
		background: none;
		box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
		background-color: rgba(255, 255, 255, 0.5);
		border-radius: 10px;
		transition: 0.4s;
		&:focus {
			box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
			background-color: rgba(255, 255, 255, 0.85);
		}
	}
}
.weather-wrap{
  color: #FFF;
.location-box {
	.location {
		color: #FFF;
		font-size: 32px;
		font-weight: 500;
		text-align: center;
    margin-bottom: 20px;
	}
	.date {
		font-size: 20px;
		font-weight: 300;
		font-style: italic;
		text-align: center;
	}
}
.weather-box {
	text-align: center;
	.temp {
		display: inline-block;
		padding: 10px 25px;
		font-size: 80px;
		font-weight: 700;		
		background-color: rgba(255, 255, 255, 0.5);
		border-radius: 16px;
		margin: 30px 0px;
		box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
	}
	.weather-type {
	
		font-size: 48px;
		font-weight: 700;
		font-style: italic;
	}
}
}
</style>
