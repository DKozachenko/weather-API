<template>
  <div class="row">
    <div class="col-12">
      <div class="weather w-50 h-50 border rounded-1 border-primary"></div>
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
  }
</style>