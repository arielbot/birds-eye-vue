<template>
  <div class="container">
    <h1>Bird's Eye Vue</h1>
    <section class="board">
      <Card
        v-for="(card, index) in cardList"
        :key="`card-${index}`"
        :matched="card.matched"
        :value="card.value"
        :visible="card.visible"
        :position="card.position"
        @select-card="flipCard"
      />
    </section>
    <h2>{{ status }}</h2>
  </div>
</template>

<script>
import { ref, watch } from 'vue'
import Card from './components/Card'
export default {
  name: 'App',
  components: {
    Card
  },
  setup() {
    const cardList = ref([])
    const userSelection = ref([])
    const status = ref('')

    for (let i = 0; i < 16; i++) {
      cardList.value.push({
        value: i,
        visible: false,
        position: i,
        matched: false
      })
    }
    const flipCard = payload => {
      cardList.value[payload.position].visible = true

      if(userSelection.value[0]){
        userSelection.value[1] = payload
      } else {
        userSelection.value[0] = payload
      }
    }

    watch(userSelection, currentValue => {
      if (currentValue.length == 2){
        const cardFirst = currentValue[0]
        const cardSecond = currentValue[1]

        // check to see if cards match
        if(cardFirst.faceValue === cardSecond.faceValue){
          status.value = "It's a match!"
          cardList.value[cardFirst.position].matched = true
          cardList.value[cardSecond.position].matched = true
        } else {
          status.value = "Sorry, try again"
          
          // flip cards back to initial position
          cardList.value[cardFirst.position].visible = false
          cardList.value[cardSecond.position].visible = false
        }

        userSelection.value.length = 0
      }
    }, { deep: true })

    return {
      cardList,
      flipCard,
      userSelection,
      status
    }
  }
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
.board {
  display: grid;
  grid-template-columns: 100px 100px 100px 100px;
  grid-template-rows: 100px 100px 100px 100px;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
  justify-content: center;
}
</style>