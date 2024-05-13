<template>
  <div class="card" @click="handleClick" :class="{ flipped: card.isFlipped }">
    <div class="card-inner">
      <div class="card-front">

        <img v-if="card.isFlipped" :src="require(`@/assets/${card.image}`)" alt="Card Image">
      </div>
      <div class="card-back">

      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    card: {
      type: Object,
      required: true
    }
  },
  methods: {
    handleClick() {
      this.$emit('click', this.card);
    }
  }
}
</script>

<style scoped>
.card-front, .card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

.card-front {
  background-color: #ccc;
  backface-visibility: hidden;
}

.card-back {
  background-color: transparent;
  transform: rotateY(180deg);
  backface-visibility: hidden;
}

.card {
  position: relative;
  width: 85px;
  height: 85px;
  perspective: 1000px;
}

.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform 0.6s;
  transform-style: preserve-3d;
}

.card.flipped .card-inner {
  transform: rotateY(180deg);
}
</style>