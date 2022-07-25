<template>
  <div id="app">
    <header>
    <h1>
      TVO Weather
    </h1>
    <form>
      <fieldset>
        <label for="query">Select a city</label>
        <input id="query" type="text" autocomplete="off" placeholder="Enter city...." @keyup="refreshCities" v-model="query">
      </fieldset>
      <input
          type="button" @click="clearQuery" value="X">
    </form>
    <ul class="results">
      <li v-for="(city, idx) in cities" :key="'part-' + idx" @click="setCity(city)">{{ city.name }}, {{ city.state }},
        {{ city.country }}
      </li>
    </ul>
    </header>
    <transition name="fade">
    <div class="selected-city" v-if="selectedCity">
      <h2>Current Weather for <strong>{{ selectedCity?.name }} {{ selectedCity?.state }} {{
          selectedCity?.country
        }}</strong></h2>
      <div>
      <WeatherSummary :weather="weather"/>
      </div>
    </div>
    </transition>
    <transition name="fade">
    <div class="empty" v-if="!selectedCity">
      <span class="material-symbols-outlined">cloud_off</span>
      <h1>Search for a city to begin</h1>
    </div>
    </transition>


    <!--    <div>-->
    <!--      <pre><code>{{weather}}</code></pre>-->
    <!--    </div>-->
  </div>
</template>

<script>

import WeatherSummary from "@/components/WeatherSummary";

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
}

body {
  margin: 0 0 0 0 ;
  background-color: #ffffff;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='4' height='4' viewBox='0 0 4 4'%3E%3Cpath fill='%23848287' fill-opacity='0.4' d='M1 3h1v1H1V3zm2-2h1v1H3V1z'%3E%3C/path%3E%3C/svg%3E");
}

header {
  background-color: darkslateblue;
  color:white;
  padding-top: 1.6em;
  padding-bottom: 1.6em;
  display: flex;
  flex-direction: column;
  align-items: center;
}

header h1 {
  font-size: 2em;
  margin-bottom: 1em;
}


form {
  text-align: left;
  display: flex;
  justify-content: center;
  align-items: center;
}

fieldset {
  display: flex;
  flex-direction: column;
  border: none;
}

label {
  padding-bottom: 0.5em;
}

input::placeholder {
  color: #cecece;
}


input[type=text] {
  padding-left: 0.4em;
  width:100%;
  height: 1.8em;
  font-size: 1.4em;
  font-weight: bold;
  border-radius: 6px;
  border: none;
  outline: #c5bef2 solid 3px;
}

input[type=button] {
  background-color: transparent; /* Green */
  border: none;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  font-weight: bold;
  font-size: 1em;
  margin-top: 1em;
  margin-left: 20px;
}

input[type=button]:hover {
  cursor: pointer;
  color: #cecece;
}

ul {
  margin-top: 1em;
  list-style: none;
}

li {
  padding-bottom: 0.4em;
}

li:hover {
  cursor: pointer;
  font-weight: bold;
}

.empty {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 3em;
  color: #cecece;
  font-size: 1.8em;
  font-weight: bold;
}

.empty span{
  font-size: 3em;
}


h1 {
  margin-top: 0;
  margin-bottom: 0;
  text-align: left;
  display: flex;
}

h2 {
  margin-top: 1em;
  margin-bottom: 1em;
  font-size: 1.8em;
  text-align: center;
}

h4 {
  text-align: left;
  width: 100%;
}

@media only screen and (max-width: 768px) {
  .lug {
    margin-bottom: 0.8em;
    width: calc(100vw - 3.2em);
  }
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

</style>
