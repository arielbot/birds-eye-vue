
<script>
export default {
  
}
</script>
<template>
  <div class="card">
    <div class="card-face is-front">
      font
    </div>
    <div class="card-face is-back">
      back
    </div>
  </div>
</template>
<style scoped>
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