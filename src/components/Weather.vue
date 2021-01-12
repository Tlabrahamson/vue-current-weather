<template>
  <div class="weather-wrapper">
    <h1>{{weather.name}}, {{weather.sys.country}}</h1>
    <img v-bind:src="`http://openweathermap.org/img/w/${weather.weather[0].icon}.png`" />
    <p>{{weather.weather[0].main}}</p>
    <h2>{{Math.ceil(weather.main.temp)}}°F</h2>
    <p>
      <span>High:</span>
      {{Math.ceil(weather.main.temp_max)}}°F
      <span>Low:</span>
      {{Math.ceil(weather.main.temp_min)}}°F
    </p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      coordinates: {
        lat: 0,
        lng: 0
      },
      weather: {}
    };
  },
  created() {
    // Get user's coordinates from browser request
    this.$getLocation({}).then(coordinates => {
      this.coordinates = coordinates;
      axios
        // Get request to the openweathermap coordinates endpoint. Passes in the latitude and longitude as variables from the current browser session.
        .get(
          `http://api.openweathermap.org/data/2.5/weather?lat=${coordinates.lat}&lon=${coordinates.lng}&units=imperial&appid=43c7d73c0d4a1c7f1c901cd07f5c556d`
        )
        .then(res => {
          this.weather = res.data;
        })
        .catch(err => {
          console.log(err);
        });
    });
  }
};
</script>

<style>
.weather-wrapper {
  font-family: "Montserrat", sans-serif;
  padding: 1rem 2rem;
  width: 300px;
  border-radius: 24px;
  background: linear-gradient(
    103.76deg,
    rgba(255, 255, 255, 0.5) 0%,
    rgba(255, 255, 255, 0.2) 100%
  );
  box-shadow: 2px 2px 20px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #2d2d2d;
}

h1 {
  font-weight: 500;
  font-size: 24px;
}

img {
  width: 100%;
  max-width: 50px;
}

h2 {
  margin: 1rem 0;
  font-size: 32px;
  font-weight: 400;
}

span {
  font-weight: 500;
}
</style>
