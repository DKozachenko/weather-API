<template>
  <div class="row">
    <div class="col-12">
      <div class="weather border rounded-1 border-primary">
        <p>{{ weatherObject.temp }}</p>
        <p>{{ weatherObject.feelsLike }}</p>
        <p>{{ weatherObject.description }}</p>
        <p>{{ weatherObject.pressure }}</p>
        <p>{{ weatherObject.windDirection }}</p>
        <p>{{ weatherObject.windSpeed }}</p>
        <img :src="weatherObject.icon" alt="icon">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    weatherObject: Object
  }
}
</script>

<style scoped>
  .weather {
    width: 300px;
    height: 150px;
    border: 1px solid black;
  }
</style>