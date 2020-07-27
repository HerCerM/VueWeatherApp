<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." />
      </div>
      <div class="weather-wrap">
        <div class="location-box">
          <div class="location">Mérida, Yucatán</div>
          <div class="date">29 de julio de 2020</div>
        </div>
        <div class="weather-box">
          <div class="temp">31°c</div>
          <div class="weather">Soleado</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      appid: "76b1e4110c2d7ceb4c599b4f96c5e43e"
    };
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
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
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

.search-box .search-bar {
  width: 100%;
  padding: 0.8em;
  display: block;
  border: none;
  outline: none;
  border-radius: 0 1em 0 1em;
  transition: all 0.3s;
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
  margin-top: 2rem;
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
  margin-top: 3rem;
  margin-bottom: 0.5rem;
  box-shadow: 0.08em 0.08em rgba(0, 0, 0, 0.2);
}

.weather-box .weather {
  font-style: italic;
  font-size: 3rem;
  text-shadow: 2px 2px rgba(0, 0, 0, 0.2);
}
</style>
