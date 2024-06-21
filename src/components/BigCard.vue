<template>
  <div class="wrapper">
    <div class="img__wrapper">
      <div class="img__left">
        <img
          @click="imgClick"
          :class="{active__img: mainPicture === card.picture}"
          :src="card.picture" />
        <img
          @click="imgClick"
          :class="{active__img: mainPicture === card.hoverPicture}"
          :src="card.hoverPicture" />
        <img
          @click="imgClick"
          :class="{active__img: mainPicture === card.img1}"
          v-if="card.img1"
          :src="card.img1" />
        <img
          @click="imgClick"
          :class="{active__img: mainPicture === card.img2}"
          v-if="card.img2"
          :src="card.img2" />
        <img
          @click="imgClick"
          :class="{active__img: mainPicture === card.img3}"
          v-if="card.img3"
          :src="card.img3" />
      </div>
      <div>
        <img class="main__img" :src="mainPicture" />
      </div>
    </div>
    <div class="text__wrapper">
      <div>
        <h2>{{ card.name }}</h2>
        <p class="inscription">{{ card.inscription }}</p>
        <p class="category">
          Category: <b>{{ card.category }}</b>
        </p>
      </div>

      <p class="disc">{{ card.discription }}</p>

      <div class="text__bottom">
        <p class="price">
          <b>{{ card.price }}</b> €
        </p>
        <div class="buttons">
          <HeartBtn
            :isFavorite="card.isFavorite"
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
  name: 'BigCard',

  data() {
    return {
      mainPicture: this.card.picture,
    };
  },

  props: {
    card: Object,
  },

  methods: {
    addToFavorite() {
      this.$emit('addToFavorite', this.card);
    },
    addToCart() {
      this.$emit('addToCart', this.card);
    },

    imgClick(img) {
      this.mainPicture = img.target.src;
    },
  },
};
</script>

<style scoped>
.wrapper {
  display: grid;
  grid-template-columns: 1fr 1fr;
  max-height: 350px;
  position: relative;
  gap: 10px;
}
.img__wrapper {
  display: flex;
}
img {
  width: 100px;
  margin: 1px 2px;
  box-sizing: border-box;
}
.img__left {
  display: flex;
  flex-direction: column;
  max-height: 354px;
  overflow-y: scroll;
  & img:hover {
    border: 1px black solid;
  }
}
.main__img {
  width: 300px;
  height: 350px;
}
.active__img {
  border: 1px black solid;
}

h2 {
  margin: 0;
  font-size: 34px;
}
p {
  margin: 0;
}
.inscription {
  font-weight: 400;
  font-size: 14px;
  color: #898989;
}
.category {
  font-size: 16px;
  margin-top: 5px;
}
.disc {
  font-size: 18px;
  margin-top: 15px;
  max-width: 400px;
}
.text__bottom {
  display: flex;
  justify-content: space-between;
  padding-right: 8px;
  & p {
    font-size: 24px;
  }
}
.price {
  position: absolute;
  bottom: 0;
}
.buttons {
  position: absolute;
  right: 0;
  bottom: 0;
}

@media (max-width: 700px) {
  .wrapper {
    grid-template-columns: 1fr;
    grid-template-rows: 1fr 1fr;
    overflow-y: scroll;
    scrollbar-gutter: stable;
    &::-webkit-scrollbar {
      display: inline-block;
      width: 4px;
    }
    &::-webkit-scrollbar-thumb {
      width: 10px;
      height: 10px;
      background-color: darkgrey;
      border-radius: 10px;
    }
  }
  .price {
    position: unset;
  }
  .buttons {
    position: unset;
  }
  img {
    width: 60px;
  }
  .img__wrapper {
    display: flex;
    flex-direction: column-reverse;
    align-items: center;
  }
  .img__left {
    flex-direction: row;
  }
  .main__img {
    height: 320px;
  }
  .disc {
    margin-bottom: 10px;
  }
}
</style>
