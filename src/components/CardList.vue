<template>
  <div v-if="cards.length === 0" class="empty">
    Sorry, but there is nothing on this request
  </div>

  <div v-else>
    <div class="wrapper">
      <TransitionGroup name="post-list">
        <Card
          @cardClick="cardClick"
          @addToFavorite="addToFavorite"
          @addToCart="addToCart"
          v-for="card in cards.slice(
            (currentPage - 1) * numberOfCards,
            currentPage * numberOfCards
          )"
          :card="card"
          :key="card.name" />
      </TransitionGroup>
    </div>

    <MyPagination
      @paginationClick="changeCurrentClick"
      :totalPage="Math.ceil(this.cards.length / numberOfCard)"
      :selectClick="selectClick" />
  </div>
</template>

<script>
export default {
  name: 'CardList',

  data() {
    return {
      currentPage: 1,
      hasChanged: this.inputChange,
      numberOfCards: this.numberOfCard,
    };
  },

  props: {
    cards: {
      type: Array,
      required: true,
    },
    numberOfCard: {
      type: Number,
      required: true,
    },
    selectClick: Number,
  },

  methods: {
    changeCurrentClick(currentPage) {
      this.currentPage = currentPage;
    },
    addToFavorite(card) {
      this.$emit('addToFavorite', card);
    },
    addToCart(card) {
      this.$emit('addToCart', card);
    },
    cardClick(card) {
      this.$emit('cardClick', card);
    },
  },

  mounted() {
    let width = document.querySelector('.wrapper').scrollWidth;
    if (width < 740 || (width > 980 && width < 1200)) {
      this.numberOfCards++;
    }
  },
};
</script>

<style scoped>
.wrapper {
  margin-top: 10px;
  display: grid;
  grid-template-columns: repeat(5, calc((100% / 5) - 10px));
  gap: 10px;
  justify-content: center;
}
@media (max-width: 1200px) {
  .wrapper {
    grid-template-columns: repeat(4, calc((100% / 4) - 8px));
    gap: 8px;
  }
}
@media (max-width: 980px) {
  .wrapper {
    grid-template-columns: repeat(3, calc((100% / 3) - 6px));
    gap: 6px;
  }
}
@media (max-width: 740px) {
  .wrapper {
    grid-template-columns: repeat(2, calc((100% / 2) - 4px));
    gap: 4px;
  }
}

.empty {
  height: 350px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 30px;
  text-align: center;
}

/* animation */
.post-list-move {
  transition: all 0.8s ease;
}
.post-list-enter-active,
.post-list-leave-active {
  transition: all 0.5s ease;
}
.post-list-enter-from,
.post-list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
