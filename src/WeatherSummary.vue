<template>
  <div id="weather-summary">
    <div class="lug">
      <h2>Conditons</h2>
      <img :src="iconSource">
    </div>
    <div class="lug">
      <h2>Current Temp</h2>
      <h1>
      {{ weather?.main?.temp }}
      </h1>
      <div class="secondary">
        <div>
          <h2>Min</h2>
          {{ weather?.main?.temp_min }}
        </div>
        <div>
          <h2>Max</h2>
          {{ weather?.main?.temp_max }}
        </div>
      </div>
    </div>
    <div class="lug">
      <h2>Humidity</h2>
      <h1>{{weather?.main?.humidity}}%</h1>
    </div>
    <div class="lug">
      <h2>Sunrise</h2>
      <h1>{{weather?.main?.humidity}}</h1>
      <h2>Sunset</h2>
      <h1>{{weather?.main?.humidity}}</h1>
    </div>
  </div>
</template>
<script>
export default {
  name: 'WeatherSummary',
  props: {
    weather: {}
  },
  computed: {
    iconSource() {
      return `http://openweathermap.org/img/wn/${this.weather?.weather[0].icon}@2x.png" alt="${this.weather?.weather[0].description}`
    }
  },
}
</script>
<style scoped>

#weather-summary {
  /*height: 6em;*/
  display: flex;
  justify-content: space-between;
}

@media only screen and (max-width: 768px) {
  #weather-summary {
    flex-direction: column;
  }
}

h1 {
  margin-top: 0;
  margin-bottom: 0;
  text-align: left;
}

h2 {
  margin-top: 0;
  margin-bottom: 0;
  font-size: 0.5em;
  text-align: left;
}

h4 {
  text-align: left;
  width: 100%;
}

.lug {
  border-radius: 0.5em;
  padding-left: 10px;
  padding-right: 10px;
  padding-top: 10px;
  background-color: #559eeb;
  color: #fff;
  width:100px;
  margin-right:1em;
  height: 8em;
}

@media only screen and (max-width: 768px) {
  .lug {
    margin-bottom: 0.8em;
    width: calc(100vw - 2.4em);
  }
}

.lug .secondary {
  display: flex;
  justify-content: space-between;
}

</style>
