<template>
  <div class="card" @click="this.$emit('cardClick', this.card)">
    <div class="photo__wrapper">
      <img :src="card.picture" alt="photo" class="photo" />
      <img :src="card.hoverPicture" alt="photo" class="hoverPhoto" />
    </div>

    <div class="cardInfo__wrapper">
      <h3>{{ card.name }}</h3>
      <h4>{{ card.inscription }}</h4>
      <div class="cardInfo__block">
        <p class="price">{{ card.price }} €</p>
        <div @click.stop>
          <HeartBtn
            :isFavorite="isFavorite"
            @click="addToFavorite"
            title="Add to Favorite" />
          <CartBtn
            @click="addToCart"
            title="Add to Cart"
            style="margin-left: 5px" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Card',
  data() {
    return {
      isFavorite: this.card.isFavorite,
    };
  },
  props: {
    card: {
      type: Object,
      required: true,
    },
  },
  methods: {
    addToFavorite() {
      this.$emit('addToFavorite', this.card);
    },
    addToCart() {
      this.$emit('addToCart', this.card);
    },
  },
};
</script>

<style scoped>
.card {
  box-shadow: 1px 3px 7px 0 rgba(0, 0, 0, 0.25);
  background: #fff;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  &:hover {
    cursor: pointer;
  }
}
img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 8px;
  vertical-align: top;
}

.photo__wrapper .hoverPhoto {
  display: none;
}
.photo__wrapper:hover .photo {
  display: none;
}
.photo__wrapper:hover .hoverPhoto {
  display: block;
}

.cardInfo__wrapper {
  display: flex;
  flex-direction: column;
  padding: 5px;
}
.cardInfo__block {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.price {
  margin: 0;
}
h3 {
  font-weight: 600;
  font-size: 24px;
  line-height: 120%;
  margin: 0;
}
h4 {
  font-weight: 400;
  font-size: 14px;
  line-height: 150%;
  color: #898989;
  margin: 0;
}
</style>

<!-- https://www.svgrepo.com/collection/furniture-line-vectors/ -->
