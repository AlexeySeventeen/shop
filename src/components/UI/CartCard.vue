<template>
  <div>
    <div class="cart__content">
      <div class="cart__left">
        <img class="cart__img" :src="cart.picture" :alt="cart.name" />
        <div class="cart__text">
          <div class="cart__top">
            <h2>{{ cart.name }}</h2>
            <p style="color: #898989">{{ cart.inscription }}</p>
            <div class="cart__bottom">
              <div class="counter">
                <button
                  @click="changeCount('minus')"
                  title="Decrement quantity"></button>
                <p>{{ count }}</p>
                <button
                  @click="changeCount('plus')"
                  title="Increment quantity"></button>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="cart__right">
        <p class="price">{{ cart.price * count }} €</p>
        <button class="cart__btn" @click="deleteCart" title="Remove from cart">
          <svg
            fill="#9e2215"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            width="20px"
            height="20px"
            viewBox="0 0 408.483 408.483"
            xml:space="preserve">
            <g>
              <g>
                <path
                  d="M87.748,388.784c0.461,11.01,9.521,19.699,20.539,19.699h191.911c11.018,0,20.078-8.689,20.539-19.699l13.705-289.316
			H74.043L87.748,388.784z M247.655,171.329c0-4.61,3.738-8.349,8.35-8.349h13.355c4.609,0,8.35,3.738,8.35,8.349v165.293
			c0,4.611-3.738,8.349-8.35,8.349h-13.355c-4.61,0-8.35-3.736-8.35-8.349V171.329z M189.216,171.329
			c0-4.61,3.738-8.349,8.349-8.349h13.355c4.609,0,8.349,3.738,8.349,8.349v165.293c0,4.611-3.737,8.349-8.349,8.349h-13.355
			c-4.61,0-8.349-3.736-8.349-8.349V171.329L189.216,171.329z M130.775,171.329c0-4.61,3.738-8.349,8.349-8.349h13.356
			c4.61,0,8.349,3.738,8.349,8.349v165.293c0,4.611-3.738,8.349-8.349,8.349h-13.356c-4.61,0-8.349-3.736-8.349-8.349V171.329z" />
                <path
                  d="M343.567,21.043h-88.535V4.305c0-2.377-1.927-4.305-4.305-4.305h-92.971c-2.377,0-4.304,1.928-4.304,4.305v16.737H64.916
			c-7.125,0-12.9,5.776-12.9,12.901V74.47h304.451V33.944C356.467,26.819,350.692,21.043,343.567,21.043z" />
              </g>
            </g>
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CartCard',

  data() {
    return {
      count: this.cart.count,
    };
  },

  props: {
    cart: Object,
  },

  methods: {
    deleteCart() {
      this.$emit('deleteFromCart', this.cart);
      this.$emit('changeTotalPrice', -this.cart.price);
    },
    changeCount(whatOperation) {
      if (whatOperation === 'plus') {
        this.count++;
        this.$emit('counterChange', this.cart.name, 'plus');
        this.$emit('changeTotalPrice', this.cart.price);
      }
      if (whatOperation === 'minus') {
        this.count--;
        this.$emit('counterChange', this.cart.name, 'minus');
        this.$emit('changeTotalPrice', -this.cart.price);
      }
      if (this.count === 0) {
        this.$emit('deleteFromCart', this.cart);
      }
    },
  },
};
</script>

<style scoped>
.cart__wrapper {
  max-height: 300px;
  overflow-y: scroll;
  padding: 0 6px;
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
.cart__content {
  box-shadow: 1px 3px 7px 0 rgba(0, 0, 0, 0.25);
  margin: 5px 0;
  padding: 2px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-radius: 10px;
}
.cart__left {
  display: flex;
  align-items: center;
}
.cart__img {
  width: 100px;
  height: 95px;
  margin-right: 12px;
}
.cart__bottom {
  display: flex;
}
h2 {
  font-weight: 600;
  font-size: 24px;
  margin: 0;
}
p {
  font-weight: 400;
  font-size: 14px;
  line-height: 150%;
  color: black;
  margin: 0;
}
.cart__right {
  width: 70px;
  height: 90px;
  position: relative;
}
.price {
  position: absolute;
  text-align: center;
  text-wrap: nowrap;
  font-size: 18px;
  top: 11px;
  right: 12px;
  width: 50px;
}
.cart__btn {
  background-color: transparent;
  border: 0;
  border-radius: 50%;
  padding: 8px 9px;
  border: 0;
  position: absolute;
  bottom: 2px;
  right: 18px;
  &:hover {
    background-color: #ff00003b;
  }
  &:focus {
    outline: 1px solid #9e2215;
  }
}

.counter {
  width: 70px;
  display: flex;
  align-items: center;
  justify-content: space-around;
  border: 1px rgb(137, 137, 137) solid;
  border-radius: 8px;
  margin-top: 2px;

  & button {
    position: relative;
    width: 16px;
    height: 16px;
    border-radius: 100%;
    border: 0;
    background-color: #92b2c1a6;
    &:first-child:after {
      content: '-';
      position: absolute;
      top: -2px;
      right: 5px;
      font-size: 17px;
    }
    &:last-child:after {
      content: '+';
      position: absolute;
      top: 0px;
      right: 3.3px;
      font-size: 16px;
    }
    &:hover {
      background-color: #92b2c1;
    }
  }
}
</style>
