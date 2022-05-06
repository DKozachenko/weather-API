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

      <div v-if="isOpenWeatherError || isWeatherError" class="row">
        <div class="col-12 text-center">
          <h3>There is no this town in at least one of town databases</h3>
        </div>
      </div>
      <div v-else-if="!isOpenWeatherError && !isWeatherError && (isLoading1ApiWeather || isLoading1ApiOpenWeather)" class="row">
        <div class="col-12 text-center">
          <h3>Loading</h3>
        </div>
      </div>
      <div v-else-if="isFirstSearch" class="row d-flex justify-content-center">
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
      API_KEY_GISMETEO: '61f2622de4e6d2.33217248',
      API_KEY_WEATHER: '79be307a625043d4aa9131638221004',
      cityName: 'London',
      isLoading1ApiOpenWeather: false,
      isLoading1ApiWeather: false,
      isOpenWeatherError: false,
      isWeatherError: false,
      isFirstSearch: false,
      weatherObjOpenWeather: {
        temp: '',
        feelsLike: '',
        description: '',
        pressure: '',
        windDirection: '',
        windSpeed: '',
        icon: '',
        fromName: 'Open Weather',
        fromLink: 'https://openweathermap.org/'
      },
      weatherObjWeather: {
        temp: '',
        feelsLike: '',
        description: '',
        pressure: '',
        windDirection: '',
        windSpeed: '',
        icon: '',
        fromName: 'Weather API',
        fromLink: 'https://www.weatherapi.com/'
      },
    }
  },
  methods: {
    async getApiOpenWeather() {
      try {
        let resp = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.cityName}&appid=${this.API_KEY_OPENWEATHER}&units=metric`)
        this.isOpenWeatherError = false
        return resp
      } catch (e) {
        this.isOpenWeatherError = true
      }
    },
    async getApiGismeteo() {
      return await axios.get(`https://api.gismeteo.net/v2/weather/current/?latitude=54.35&longitude=52.52`, {
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Access-Control-Allow-Headers': 'Origin, X-Requested-With, Content-Type, Accept',
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
      try {
        let resp = await axios.get(`http://api.weatherapi.com/v1/current.json?key=${this.API_KEY_WEATHER}&q=${this.cityName}&aqi=no`)
        this.isWeatherError = false
        return resp
      } catch (e) {
        this.isWeatherError = true
      }

    },
    fillOpenWeatherObj(response) {
      this.weatherObjOpenWeather = {
        ...this.weatherObjOpenWeather,
        temp: response.data.main.temp.toFixed(0),
        feelsLike: response.data.main.feels_like.toFixed(0),
        description: response.data.weather[0].description[0].toUpperCase() + response.data.weather[0].description.slice(1),
        pressure: (response.data.main.pressure * 0.750062).toFixed(0) ,
        windDirection: this.processWindDirection(response.data.wind.deg),
        windSpeed: response.data.wind.speed.toString(),
        icon: `https://openweathermap.org/img/wn/${response.data.weather[0].icon}.png`
      }
    },
    fillWeatherObj(response) {
      this.weatherObjWeather = {
        ...this.weatherObjWeather,
        temp: response.data.current.temp_c.toFixed(0),
        feelsLike: response.data.current.feelslike_c.toFixed(0),
        description: response.data.current.condition.text,
        pressure: (response.data.current.pressure_mb * 0.750062).toFixed(0),
        windDirection: this.processWindDirection(response.data.current.wind_degree),
        windSpeed: (response.data.current.wind_mph * 5 / 18).toFixed(2).toString(),
        icon: response.data.current.condition.icon
      }
    },
    processWindDirection(deg) {
      if (deg > 348 && deg <= 360 || deg >= 0 && deg <= 11) {
        return "N"
      }
      if (deg > 11 && deg <= 33) {
        return "NNE"
      }
      if (deg > 33 && deg <= 56) {
        return "NE"
      }
      if (deg > 56 && deg <= 78) {
        return "ENE"
      }
      if (deg > 78 && deg <= 101) {
        return "E"
      }
      if (deg > 101 && deg <= 123) {
        return "ESE"
      }
      if (deg > 123 && deg <= 146) {
        return "SE"
      }
      if (deg > 146 && deg <= 168) {
        return "SSE"
      }
      if (deg > 168 && deg <= 191) {
        return "S"
      }
      if (deg > 191 && deg <= 213) {
        return "SSW"
      }
      if (deg > 213 && deg <= 236) {
        return "SW"
      }
      if (deg > 236 && deg <= 258) {
        return "WSW"
      }
      if (deg > 258 && deg <= 281) {
        return "W"
      }
      if (deg > 281 && deg <= 303) {
        return "WNW"
      }
      if (deg > 303 && deg <= 326) {
        return "NW"
      }
      if (deg > 326 && deg <= 348) {
        return "NNW"
      }
    },
    //по долготе и широте
    async getAllData() {
      const forecast1 = await this.getApiOpenWeather()
      if (!this.isOpenWeatherError) {
        this.isLoading1ApiOpenWeather = false
        this.isFirstSearch = true
        this.fillOpenWeatherObj(forecast1)
      }

      //const forecast2 = await this.getApiYandex()
      //console.log(forecast2)
      //const forecast3 = await this.getApiGismeteo()
      //console.log(forecast3)
      const forecast4 = await this.getApiWeather()
      if (!this.isWeatherError) {
        this.isLoading1ApiWeather = false
        this.isFirstSearch = true
        this.fillWeatherObj(forecast4)
      }
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