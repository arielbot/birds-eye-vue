<script>
export default {
  props: {
    matched: {
      type: Boolean,
      default: false
    },
    position: {
      type: Number,
      required: true
    },
    value: {
      type: Number,
      required: true
    },
    visible: {
      type: Boolean,
      default: false
    }
  },
  setup(props, context) {
    const selectCard = () => {
      context.emit('select-card', {
        position: props.position,
        faceValue: props.value
      })
    }
    return {
      selectCard
    }
  }
}
</script>

<template>
  <div class="card" @click="selectCard">
    <div v-if="visible" class="card-face front">
      {{ value }} - {{ matched }}
    </div>
    <div v-else class="card-face back">
      Back
    </div>
  </div>
</template>

<style>
.card {
  border: 5px solid #ccc;
  position: relative;
}
.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
  color: white;
}
.card-face.front {
  background-color: red;
}
.card-face.back {
  background-color: blue;
}
</style>