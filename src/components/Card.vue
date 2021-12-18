<script>
export default {
  props: {
    value: {
      type: Number,
      required: true
    }
  }
}
</script>

<template>
  <div class="card">
    {{ value }}
  </div>
</template>

<style>
  .card {
    border: 5px solid gray;
    position: relative;
  }
  .card-face {
    width: 100%;
    height: 100%;
    color: white;
    position: absolute;
  }
  .card-face.is-front {
    background-color: red;
  }
  .card-face.is-back {
    background-color: blue;
  }
</style>