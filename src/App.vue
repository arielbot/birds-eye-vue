<template>
  <div class="container">
    <h1 id="app">Bird's Eye Vue</h1>
    <section class="main-board">
      <Card v-for="item in size" :key="item"
      />
    </section>
  </div>
</template>

<script>
import Card from './components/Card'
export default {
  
  // duplicate card component
  data () {
    return {
      size: 16
    }
  },

  name: 'App',
  components: {
    Card
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
  margin-top: 60px;
}
.main-board {
  display: grid;
  grid-template-columns: 100px 100px 100px 100px;
  grid-template-rows: 100px 100px 100px 100px;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
  justify-content: center;
}
</style>
