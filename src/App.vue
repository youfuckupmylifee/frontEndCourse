<template>
  <div id="app">
    <h1>Memory Game</h1>
    <div class="game-board">
      <game-card
        v-for="card in cards"
        :key="card.id"
        :card="card"
        @click="handleCardClick(card)"
      />
    </div>
  </div>
</template>

<script>
import GameCard from './GameCard.vue';

export default {
  components: {
    GameCard
  },
  data() {
    return {
      cards: this.setupCards(),
      flippedCards: [],
      matchedPairs: 0,
    };
  },
  methods: {
    setupCards() {
      const images = [
        'image1.png', 'image2.png', 'image3.png', 'image4.png',
        'image5.png', 'image6.png', 'image7.png', 'image8.png',
      ];
      let doubledCards = images.concat(images).map((image, index) => ({
        id: index,
        image,
        isFlipped: false,
        isMatched: false,
      }));

      for (let i = doubledCards.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [doubledCards[i], doubledCards[j]] = [doubledCards[j], doubledCards[i]];
      }
      return doubledCards;
    },
    handleCardClick(card) {
      if (card.isFlipped || this.flippedCards.length === 2) {
        return;
      }

      card.isFlipped = true;
      this.flippedCards.push(card);

      if (this.flippedCards.length === 2) {
        this.checkMatch();
      }
    },
    checkMatch() {
      const [firstCard, secondCard] = this.flippedCards;

      if (firstCard.image === secondCard.image) {
        firstCard.isMatched = true;
        secondCard.isMatched = true;
        this.matchedPairs++;
        if (this.matchedPairs === this.cards.length / 2) {
          alert('Congratulations! You have matched all pairs.');
        }
        this.resetFlippedCards();
      } else {
        setTimeout(() => {
          firstCard.isFlipped = false;
          secondCard.isFlipped = false;
          this.resetFlippedCards();
        }, 1000);
      }
    },
    resetFlippedCards() {
      this.flippedCards = [];
    }
  }
}
</script>

<style>
#app {
  text-align: center;
  margin-top: 50px;
}
.game-board {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 10px;
  max-width: 640px;
  margin: auto;
}
</style>
