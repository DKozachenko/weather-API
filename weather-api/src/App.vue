<template>
  <main>
    <div class="container">
      <div class="row mb-3">
        <div class="col-12 text-center">
          <h1>Whether API Service</h1>
        </div>
      </div>

      <div class="row mb-3 d-flex justify-content-center">
        <div class="col-6 d-flex">
          <div class="input-group mr-4">
            <span class="input-group-text" id="basic-addon1">City: </span>
            <input v-model="cityName" type="text" class="form-control" placeholder="" aria-label="Username" aria-describedby="basic-addon1">
          </div>

          <button type="button" class="btn btn-primary" @click="getAllData">Get</button>
        </div>
      </div>

      <div v-if="isLoading1ApiWeather || isLoading1ApiOpenWeather" class="row">
        <div class="col-12 text-center">
          <h3>Loading</h3>
        </div>
      </div>
      <div v-else class="row">
        <div class="col-4">
          <weather-view :weather-object="weatherObjOpenWeather"></weather-view>
        </div>
        <div class="col-4">
          <weather-view :weather-object="weatherObjWeather"></weather-view>
        </div>
      </div>
    </div>

  </main>
</template>

<script>
import WeatherView from "@/components/WeatherView";
import axios from "axios";

export default {
  components: {WeatherView},
  data() {
    return {
      API_KEY_OPENWEATHER: '3f46e27510c3107fecb861b36d0bf1a0',
      API_KEY_YANDEX: 'b86473f6-f162-4473-84e3-086c02e35068',
      API_KEY_GISMETEO: '56b30cb255.3443075',
      API_KEY_WEATHER: '79be307a625043d4aa9131638221004',
      cityName: 'London',
      isLoading1ApiOpenWeather: false,
      isLoading1ApiWeather: false,
      weatherObjOpenWeather: {
        temp: '',
        feelsLike: '',
        description: '',
        pressure: '',
        windDirection: '',
        windSpeed: '',
        icon: ''
      },
      weatherObjWeather: {
        temp: '',
        feelsLike: '',
        description: '',
        pressure: '',
        windDirection: '',
        windSpeed: '',
        icon: ''
      },
    }
  },
  methods: {
    async getApiOpenWeather() {
      this.isLoading1ApiOpenWeather = true
      return await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.cityName}&appid=${this.API_KEY_OPENWEATHER}&units=metric`)
    },
    async getApiGismeteo() {
      return await axios.get(`https://api.gismeteo.net/v2/weather/current/?latitude=54.35&longitude=52.52`, {
        headers: {
          'Access-Control-Allow-Origin': '*',
          'X-Gismeteo-Token': this.API_KEY_GISMETEO,
        },

      })
    },
    async getApiYandex() {
      return await axios.get(`https://api.weather.yandex.ru/v2/informers?lat=54.35&lon=52.52`, {
        headers: {
          'Access-Control-Allow-Origin': '*',
          'X-Yandex-API-Key': this.API_KEY_YANDEX,
        },

      })
    },
    async getApiWeather() {
      this.isLoading1ApiWeather = true
      return await axios.get(`http://api.weatherapi.com/v1/current.json?key=${this.API_KEY_WEATHER}&q=${this.cityName}&aqi=no`)
    },
    fillOpenWeatherObj(response) {

      this.weatherObjOpenWeather = {
        temp: response.data.main.temp,
        feelsLike: response.data.main.feels_like,
        description: response.data.weather[0].description[0].toUpperCase() + response.data.weather[0].description.slice(1),
        pressure: (response.data.main.pressure * 0.750062).toFixed(0) ,
        windDirection: response.data.wind.deg.toString(),
        windSpeed: response.data.wind.speed.toString(),
        icon: `https://openweathermap.org/img/wn/${response.data.weather[0].icon}.png`
      }
    },
    fillWeatherObj(response) {
      this.weatherObjWeather = {
        temp: response.data.current.temp_c,
        feelsLike: response.data.current.feelslike_c,
        description: response.data.current.condition.text,
        pressure: (response.data.current.pressure_mb * 0.750062).toFixed(0),
        windDirection: response.data.current.wind_degree.toString(),
        windSpeed: (response.data.current.wind_mph * 5 / 18).toFixed(2).toString(),
        icon: response.data.current.condition.icon
      }
    },
    //по долготе и широте
    async getAllData() {
      const forecast1 = await this.getApiOpenWeather()
      this.isLoading1ApiOpenWeather = false
      this.fillOpenWeatherObj(forecast1)
      console.log(this.weatherObjOpenWeather)
      //const forecast2 = await this.getApiYandex()
      //console.log(forecast2)
      //const forecast3 = await this.getApi3()
      //console.log(forecast3)
      const forecast4 = await this.getApiWeather()
      this.isLoading1ApiWeather = false
      this.fillWeatherObj(forecast4)
      console.log(this.weatherObjWeather)
    }
  },
}
</script>

<style scoped>
  main {
    padding: 40px 0;
    height: 100%;
    min-height: 100vh;
    background: lightsalmon;
  }
</style>