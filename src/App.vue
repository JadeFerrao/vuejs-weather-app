<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Enter Location"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <!-- Add the weather icon and text here -->
      <div class="weather-app-text" v-if="typeof weather.main == 'undefined'">Weather App</div> 

      <div class="weather-wrap" v-if="typeof weather.main !== 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ weather.dt }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="humidity">Humidity: {{ weather.main.humidity }}%</div>
          <div class="wind-speed">Wind Speed: {{ weather.wind.speed }} m/s</div>
          <div class="wind-direction">Wind Direction: {{ weather.wind.deg }}°</div>
          <div class="visibility">Visibility: {{ weather.visibility }} m</div>
        </div>
      </div>
     
      <div class="error-weather-wrap" v-else-if="weather.cod == 404" >{{ weather.message }}</div>

    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "d102ce6f8a7f8c61a416505fdeb98697",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      weatherBackgrounds: {
        Clear: "sunny-bg.jpg",
        Rain: "rainy-bg.jpg",
        Clouds: "cloudy-bg.jpg",
        Snow: "snow-bg.jpg",
        Thunderstorm: "thunder-bg.jpg",
        Mist: "mist-bg.jpg",
        Tornado: "tornado-bg.jpg",
        Haze: "haze-bg.jpg",
        Fog: "fog-bg.jpg",
      },
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key === "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`,
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      console.log(results);
      results.dt = new Date(results.dt * 1000).toDateString();
      this.weather = results;
      console.log(this.weather);

      // Check if the weather condition exists in the mapping

      if (
        results.weather[0] &&
        results.weather[0].main in this.weatherBackgrounds
      ) {
        //console.log(this.$el, "before");
        const condition = results.weather[0].main;
        // Update the background image class based on the weather condition
        //this.$el.className = condition.toLowerCase();
        this.$el.classList.remove(this.$el.classList[0]);
        this.$el.classList.add(condition.toLowerCase());
        //console.log(this.$el, "after");
        //console.log("tag", this.$el.classList[0]);
      }
    },
  },
};
</script>

<style>
.weather-icon {
  text-align: center;
}

.weather-icon img {
  width: 60px; /* Adjust the width as needed */
}

.weather-app-text {
  font-size: 24px;
  color: aqua; /* Change the text color as desired */
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin-top: 380px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5); /* Add a text shadow for a cool effect */
}

.error-weather-wrap{
font-size: 24px;
  color: red; /* Change the text color as desired */
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin-bottom: 480px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5); /* Add a text shadow for a cool effect */
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  border-radius: 10px;
}

#app.clear {
  background-image: url("./assets/sunny-bg.jpg");
  background-size: cover;
  background-position: center;
}

#app.rain {
  background-image: url("./assets/rainy-bg.jpg");
  background-size: cover;
}

#app.clouds {
  background-image: url("./assets/cloudy-bg.jpg");
  background-size: cover;
  background-position: center;
}

#app.snow {
  background-image: url("./assets/snow-bg.jpg");
  background-size: cover;
  background-position: center;
}

#app.thunderstorm {
  background-image: url("./assets/thunder-bg.jpg");
  background-size: cover;
  background-position: center;
}

#app.haze {
  background-image: url("./assets/haze-bg.jpg");
  background-size: cover;
}

#app.mist {
  background-image: url("./assets/mist-bg.jpg");
  background-size: cover;
  background-position: center;
}

#app.haze {
  background-image: url("./assets/haze-bg.jpg");
  background-size: cover;
}

#app.fog {
  background-image: url("./assets/fog-bg.jpg");
  background-size: cover;
  background-position: center;
}

#app.tornado {
  background-image: url("./assets/tornado-bg.jpeg");
  background-size: cover;
  background-position: center;
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.15),
    rgba(0, 0, 0, 0.25)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

::placeholder {
  color: black;
  opacity: 0.5;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: black;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.5);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .humidity {
  color: #fff;
  font-size: 15px; /* Adjust the font size to make it smaller */
  font-weight: 400;
  text-shadow: 1px 2px rgba(0, 0, 0, 0.25);
  flex: 1;
  text-align: left; /* Align the text to the left */
  display: inline-block; /* Make it inline-block to align on the same line */
  margin-right: 25px; /* Add margin to create a gap between humidity and wind speed */
  margin-top: 20px; /* Adjust margin as needed */
  font-weight: 700;
}

.weather-box .wind-speed {
  color: #fff;
  font-size: 15px; /* Adjust the font size to make it smaller */
  font-weight: 400;
  text-shadow: 1px 2px rgba(0, 0, 0, 0.25);
  flex: 1;
  text-align: right;
  display: inline-block; /* Make it inline-block to align on the same line */
  margin-top: 20px; /* Adjust margin as needed */
  font-weight: 700;
}


.weather-box .wind-direction {
  color: #fff;
  font-size: 15px; /* Adjust the font size to make it smaller */
  font-weight: 400;
  text-shadow: 1px 2px rgba(0, 0, 0, 0.25);
  flex: 1;
  text-align: left; /* Align the text to the left */
  display: inline-block; /* Make it inline-block to align on the same line */
  margin-right: 25px; /* Add margin to create a gap between humidity and wind speed */
  margin-top: 20px; /* Adjust margin as needed */
  font-weight: 700;
}

.weather-box .visibility {
  color: #fff;
  font-size: 15px; /* Adjust the font size to make it smaller */
  font-weight: 400;
  text-shadow: 1px 2px rgba(0, 0, 0, 0.25);
  flex: 1;
  text-align: right;
  display: inline-block; /* Make it inline-block to align on the same line */
  margin-top: 20px; /* Adjust margin as needed */
  font-weight: 700;
}
</style>
