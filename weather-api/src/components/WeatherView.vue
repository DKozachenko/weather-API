<template>
  <div class="row">
    <div class="col-12">
      <div class="weather border rounded rounded-3 border-dark py-2 px-4">
        <div class="top">
          <div class="top__img">
            <img :src="weatherObject.icon" alt="icon">
          </div>
          <p>{{ weatherObject.temp > 0 ? '+' : '' }}{{ weatherObject.temp }}</p>
          <p>{{ weatherObject.feelsLike > 0 ? '+' : '' }}{{ weatherObject.feelsLike }}<br> По ощущениям </p>
        </div>

        <div class="middle">
          <div class="middle__left">
            <p>{{ weatherObject.description }}</p>
            <p>{{ weatherObject.pressure }} мм.рт.ст.</p>
          </div>
          <div class="middle__right">
            <p>{{ weatherObject.windDirection }}</p>
            <p>{{ weatherObject.windSpeed }} м/с</p>
          </div>
        </div>

        <div class="down">
          <a v-bind:href="weatherObject.fromLink">{{ weatherObject.fromName }}</a>
        </div>
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
  .top {
    display: flex;
    justify-content: space-between;
    align-items: center;

    margin-bottom: 15px;
  }

  .top__img {
    width: 55px;
  }

  .top__img img {
    width: 100%;
  }

  .top p {
    font-size: 25px;
    margin-bottom: 0;
  }

  .top p:last-child {
    font-size: 14px;
  }

  .middle {
    display: flex;
    justify-content: space-between;

    margin-bottom: 15px;
  }

  .middle__left,
  .middle__right {
    flex-basis: 45%;
  }

  .middle__left p,
  .middle__right p {
    margin-bottom: 8px;
  }

  .down {
    display: flex;
    justify-content: flex-end;
  }
</style>