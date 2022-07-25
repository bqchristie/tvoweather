<template>
  <div id="app">
    <h1>
      TVO Weather
    </h1>
    <input type="text" placeholder="Enter city...." @keyup="refreshCities" v-model="query"><input
      type="button" @click="clearQuery" value="Clear">
    <ul class="results">
      <li v-for="(city, idx) in cities" :key="'part-' + idx" @click="setCity(city)">{{ city.name }}, {{ city.state }},
        {{ city.country }}
      </li>
    </ul>
    <div v-if="selectedCity">
      <h2>Current Weather for {{ selectedCity?.name }} {{ selectedCity?.state }} {{ selectedCity?.country }}</h2>
      <WeatherSummary :weather="weather"/>
    </div>

    <div v-if="!selectedCity">
      <h2>Select a city from the text box above</h2>
    </div>

<!--    <div>-->
<!--      <pre><code>{{weather}}</code></pre>-->
<!--    </div>-->
  </div>
</template>

<script>

import WeatherSummary from "@/WeatherSummary";

export default {
  name: 'App',
  components: {WeatherSummary},
  data() {
    return {
      cities: [],
      selectedCity: null,
      weather: null,
      units: 'metric',
      query: null,
    }
  },
  methods: {

    async refreshCities() {
      console.log("refreshing cities")
      console.log(this.query)

      const cityData = await fetch(`https://api.openweathermap.org/geo/1.0/direct?q=${this.query}&limit=5&appid=bf89e941da14d9a5e0a60764ed99bf4a`)
      this.cities = await cityData.json()
      console.log(cityData)
    },

    async refreshWeatherData() {
      const url =
          `https://api.openweathermap.org/data/2.5/weather?lat=${this.selectedCity.lat}&lon=${this.selectedCity.lon}&appid=bf89e941da14d9a5e0a60764ed99bf4a&units=${this.units}`
      const weatherData = await fetch(url)
      this.weather = await weatherData.json()
      console.log(url)
    },

    setCity(city) {
      this.selectedCity = city;
      this.cities = []
    },

    clearQuery() {
      this.query = null
      this.setCity(null)
    }


  },
  watch: {
    selectedCity(newval) {
      newval ? this.refreshWeatherData() : this.weather = null
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

input {
  height: 1.8em;
  font-size: 1.6em;
}
</style>
