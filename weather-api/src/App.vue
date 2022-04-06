<template>
  <main>
    <div class="container">
      <div class="row mb-3">
        <div class="col-12 text-center">
          <h1>Whether API Service</h1>
        </div>
      </div>

      <div class="row mb-3 d-flex justify-content-center">
        <div class="col-6">
          <div class="input-group">
            <span class="input-group-text" id="basic-addon1">City: </span>
            <input type="text" class="form-control" placeholder="" aria-label="Username" aria-describedby="basic-addon1">
          </div>
        </div>
      </div>

      <weather-view :weather-object="weatherObj1"></weather-view>
    </div>

  </main>
</template>

<script>
import WeatherView from "@/components/WeatherView";
export default {
  components: {WeatherView},
  data() {
    return {
      isLoading1Api: false,
      weatherObj1: {
        currentWeather: 45,
        veter: '45 cvfss'
      }
    }
  }
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