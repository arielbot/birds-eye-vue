<template>
  <div class="container">
    <main>
      <h1>Bird's Eye Vue</h1>
      <section>
        <h2>Pick a bird. Any bird.</h2>
      </section>
      <transition-group tag="section" class="board" name="shuffle-effect">
        <Card
          v-for="card in cardList"
          :key="`${card.value}-${card.variant}`"
          :matched="card.matched"
          :value="card.value"
          :visible="card.visible"
          :position="card.position"
          @select-card="flipCard"
        />
      </transition-group>
      <h3>{{ status }}</h3>
      <div class="grid-container">
        <button class="confetti" @click="fireConfetti" role="button">I Wanna Celebrate</button>
        <button class="restart" @click="restartGame" role="button">Restart Game</button>
      </div>
    </main>
    <Footer />
  </div>
</template>

<script>
import _ from 'lodash' 
import { computed, ref, watch } from 'vue'
import { celebrateMode } from './utilities/confetti'
import Card from './components/Card'
import Footer from './components/Footer'
export default {
  name: 'App',
  components: {
    Card,
    Footer
  },
  setup() {
    const cardList = ref([])
    const userSelection = ref([])
    
    // track user's game
    const status = computed(() => {
      // need to unpack and add .value because it is reactive
      if(pairs.value === 0){
        return 'You win!'
      } else {
        return `Remaining pairs: ${pairs.value}`
      }
    })
    
    // check how many unmatched pairs are remaining
    const pairs = computed(() => {
      const remainingCards = cardList.value.filter(card => card.matched === false).length
      return remainingCards / 2
    })

    const restartGame = () => {
      cardList.value = _.shuffle(cardList.value)

      cardList.value = cardList.value.map((card, index) => {
        return {
          ...card,
          matched: false,
          position: index,
          visible: false
        }
      })
    }

    // just give me the confetti
    const fireConfetti = () => {
      celebrateMode()
    }

    // create matching card pair
    const cardItems = [
      'cardinal',
      'chicken',
      'crane',
      'dove',
      'falcon',
      'kiwi',
      'penguin',
      'sparrow'
      ]
    
    cardItems.forEach(item => {
      cardList.value.push({
        value: item,
        variant: 1,
        visible: false,
        position: null,
        matched: false
      })
      cardList.value.push({
        value: item,
        variant: 2,
        visible: false,
        position: null,
        matched: false
      })
    })

    cardList.value = cardList.value.map((card, index) => {
      return {
        ...card,
        position: index
      }
    })
    
    const flipCard = payload => {
      cardList.value[payload.position].visible = true

      if(userSelection.value[0]){
        if((userSelection.value[0].position === payload.position) && (userSelection.value[0].faceValue === payload.faceValue)){
          return
        } else {
          userSelection.value[1] = payload
        }
      } else {
        userSelection.value[0] = payload
      }
    }

    /* TODO randomly pick a card
    const randomCard = payload => {
      cardList.value = _.sample(cardList.value)
      cardList.value[payload.position].visible = true
    } */

    watch(pairs, currentValue => {
      if(currentValue === 0) {
        celebrateMode()
      }
    })

    watch(userSelection, currentValue => {
      if (currentValue.length == 2){
        const cardFirst = currentValue[0]
        const cardSecond = currentValue[1]

        // check to see if cards match
        if(cardFirst.faceValue === cardSecond.faceValue){
          
          cardList.value[cardFirst.position].matched = true
          cardList.value[cardSecond.position].matched = true

        } else {
          
          // flip cards back to initial position
          setTimeout(() => {
            cardList.value[cardFirst.position].visible = false
            cardList.value[cardSecond.position].visible = false
          }, 1000)
        }

        userSelection.value.length = 0
      }
    }, { deep: true })

    return {
      cardList,
      flipCard,
      userSelection,
      status,
      fireConfetti,
      restartGame
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Droid+Serif|Roboto:300');
#app {
  font-family: 'Roboto', Sans-Serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

main {
  min-height: 750px;
}

.container {
  position: relative;
  min-height: 90vh;
}

.board {
  display: grid;
  grid-template-columns: repeat(4, 100px);
  grid-template-rows: repeat(4, 100px);
  grid-column-gap: 20px;
  grid-row-gap: 20px;
  justify-content: center;
}

.grid-container {
  display: grid;
  grid-template-columns: auto auto;
  grid-template-rows: auto auto;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
  justify-content: center;
}

h2 {
  font-size: 20px;
  margin-bottom: 30px;
  color: #405B72;
}

button {
  font-family: 'Roboto', Sans-Serif;
  align-items: center;
  border: 2px solid #111;
  border-radius: 8px;
  box-sizing: border-box;
  color: #111;
  cursor: pointer;
  display: flex;
  font-size: 16px;
  height: 48px;
  justify-content: center;
  line-height: 24px;
  max-width: 100%;
  padding: 0 25px;
  position: relative;
  text-align: center;
  text-decoration: none;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  margin: 0 auto;
}

button.confetti {
  background-color: #DFEAF6;
}

button.restart {
  background-color: #dde6dd;
}

button:after {
  background-color: #111;
  border-radius: 8px;
  content: "";
  display: block;
  height: 48px;
  left: 0;
  width: 100%;
  position: absolute;
  top: -2px;
  transform: translate(8px, 8px);
  transition: transform .2s ease-out;
  z-index: -1;
}

button:hover:after {
  transform: translate(0, 0);
}

button:hover {
  outline: 0;
}

.shuffle-effect-move {
  transition: transform 0.8s ease-in;
}

@media (min-width: 768px) {

  button {
    padding: 0 40px;
  }

}
@media screen and (max-width: 500px) {

  main {
    min-height: unset;
  }

  .board {
    grid-template-columns: repeat(4, 70px);
    grid-template-rows: repeat(4, 70px);
    grid-column-gap: 10px;
    grid-row-gap: 10px;
  }

}
</style>