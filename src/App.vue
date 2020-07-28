<template>
  <div id="app" :class="coldOrWarm()">
    <main>
      <div class="search-box">
        <input
          @keyup.enter="fetchWeather"
          v-model="city"
          type="text"
          class="search-bar"
          placeholder="Search..."
        />
        <transition name="fade">
          <div v-show="locNotFound" class="error-msg">
            No city, country or state matched your query
          </div>
        </transition>
      </div>
      <div class="weather-wrap" v-if="weatherData">
        <div class="location-box">
          <div class="location">{{ weatherData.city }}</div>
          <div class="date">{{ todayDate() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">
            {{ weatherData.temp | round | posfixTemp(this.unitSystem) }}
          </div>
          <div class="weather">{{ weatherData.weather }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "app",
  data() {
    return {
      city: "",
      locNotFound: false,
      weatherData: undefined,
      unitSystem: "metric",
      appid: "76b1e4110c2d7ceb4c599b4f96c5e43e",
      baseUrl: "http://api.openweathermap.org/data/2.5/weather"
    };
  },
  methods: {
    fetchWeather() {
      axios
        .get(
          `${this.baseUrl}?q=${this.city}&units=${this.unitSystem}&appid=${this.appid}`
        )
        .then(res => {
          console.log(JSON.stringify(res));
          const city = res.data.name;
          const temp = res.data.main.temp;
          const weather = res.data.weather[0].main;
          this.weatherData = { city, temp, weather };
        })
        .catch(err => {
          this.showErrorMsg();
        });
      this.city = "";
    },
    showErrorMsg() {
      this.locNotFound = true;
      setTimeout(() => {
        this.locNotFound = false;
      }, 3000);
    },
    coldOrWarm() {
      if (this.weatherData) {
        return this.weatherData.temp >= 25 ? "warm" : "cold";
      }
      return "cold";
    },
    todayDate() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December"
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday"
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  },
  filters: {
    posfixTemp(temp, units) {
      let posfix = "";
      if (units === "metric") {
        posfix = "°c";
      } else if (units == "imperial") {
        posfix = "°f";
      }
      return temp + posfix;
    },
    round(num) {
      return Math.round(num);
    }
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Source+Sans+Pro&display=swap");

* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

body {
  font-family: "Source Sans Pro", sans-serif;
}

#app {
  background-size: cover;
  background-position: top;
  transition: 0.4s;
}

#app.cold {
  background-image: url("./assets/cold-bg.jpg");
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}

main {
  min-height: 100vh;
  padding: 1.5rem;
  background: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.2),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  position: relative;
  top: 0;
}

.search-box .search-bar {
  width: 100%;
  padding: 0.8em;
  display: block;
  border: none;
  outline: none;
  border-radius: 0 1em 0 1em;
  transition: all 0.3s;
  font-size: 1.3rem;
  background-color: rgba(255, 255, 255, 0.35);
}

.search-box .search-bar:hover {
  background-color: rgba(255, 255, 255, 0.65);
  // Use rgba for shadows!!
  box-shadow: 0 3px 5px 2px rgba(0, 0, 0, 0.2);
  border-radius: 1em 0 1em 0;
}

// Notice text-align and color are inherited
.location-box {
  text-align: center;
  color: #fff;
}

.location-box .location {
  margin-top: 5.5rem;
  font-weight: 400;
  font-size: 2.3rem;
  text-shadow: 2px 2px rgba(0, 0, 0, 0.2);
}

.location-box .date {
  font-size: 1.1em;
  font-weight: 100;
  font-style: italic;
}

.weather-box {
  text-align: center;
  color: #fff;
}

.weather-box .temp {
  display: inline-block;
  padding: 0.3em;
  background-color: rgba(255, 255, 255, 0.35);
  border-radius: 0.3em;
  font-size: 4rem;
  font-weight: 1000;
  text-shadow: 0.05em 0.05em rgba(0, 0, 0, 0.2);
  margin-top: 4rem;
  margin-bottom: 1.5rem;
  box-shadow: 0.08em 0.08em rgba(0, 0, 0, 0.2);
}

.weather-box .weather {
  font-style: italic;
  font-size: 3rem;
  text-shadow: 2px 2px rgba(0, 0, 0, 0.2);
}

// Error message

.error-msg {
  position: absolute;
  left: 0;
  bottom: 0;
  transform: translateY(115%);
  background-color: #db4035;
  padding: 0.8em;
  border-radius: 0.3em;
  color: #fff;
}

// Duration of both enter and leave transitions
.fade-enter-active,
.fade-leave-active {
  transition: 0.4s;
}

// Move down on enter
.fade-enter {
  transform: translateY(2em);
}

// Opacity cero on enter and leave
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
