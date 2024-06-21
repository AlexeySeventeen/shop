<template>
  <div class="wrapper">
    <!-- empty -->
    <div v-if="cartArray.length === 0" class="emptyArray">
      <h2>Now cart is empty</h2>
    </div>

    <div v-else>
      <!-- cards -->
      <div v-if="!endShoppingActive">
        <transition-group class="transition" name="list" tag="p">
          <div class="cart__wrapper">
            <CartCard
              v-for="cart in cartArray"
              :cart="cart"
              :key="cart.name"
              @counterChange="
                (name, sigh) => {
                  this.$emit('counterChange', name, sigh);
                }
              "
              @changeTotalPrice="changeTotalPrice"
              @deleteFromCart="deleteFromCart" />
          </div>
        </transition-group>

        <div class="cart__final">
          <div>
            <h2>Total price:</h2>
            <h2>{{ this.totalPrice }} €</h2>
          </div>

          <button
            v-if="!authorization"
            class="cart__btn-final"
            @click="this.$emit('checkoutFalse')"
            title="Go to payment">
            Checkout
          </button>

          <button
            v-else
            class="cart__btn-final"
            @click="checkoutClick"
            title="Go to payment">
            Checkout
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CartActive',

  data() {
    return {
      totalPrice: 0,
      newOrderInfo: {
        date: null,
        totalPrice: 0,
        products: [],
      },
    };
  },
  props: {
    cartArray: Array,
    authorization: Boolean,
  },

  methods: {
    changeTotalPrice(change) {
      this.totalPrice = this.totalPrice + change;
    },
    deleteFromCart(cartName) {
      this.$emit('deleteFromCart', cartName);
    },

    checkoutClick() {
      let date = new Date();
      let dateNow;
      if (date.getMonth() + 1 < 10) {
        dateNow = `${date.getDate()}.0${
          date.getMonth() + 1
        }.${date.getFullYear()}`;
      } else {
        dateNow = `${date.getDate()}.${
          date.getMonth() + 1
        }.${date.getFullYear()}`;
      }
      this.newOrderInfo.totalPrice = this.totalPrice;
      this.newOrderInfo.date = dateNow;
      this.newOrderInfo.products = this.cartArray;

      this.$emit('checkout', this.newOrderInfo);
    },
  },

  mounted() {
    this.cartArray.forEach((cart) => {
      this.totalPrice = this.totalPrice + (+cart.price * +cart.count);
    });
  },
};
</script>

<style scoped>
.cart__wrapper {
  height: 320px;
  margin-top: 5px;
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

.cart__final {
  margin-top: 10px;
  & div {
    display: flex;
    justify-content: space-between;
  }
}
h2 {
  font-weight: 600;
  font-size: 24px;
  margin: 0;
}
.cart__btn-final {
  position: relative;
  margin-top: 10px;
  width: 100%;
  padding: 8px;
  border-radius: 8px;
  border: 0;
  font-weight: 600;
  font-size: 15px;
  background-color: #9ccce3;
  &:hover {
    background-color: #7cbad7;
  }
  &:after {
    content: url(./all-svg/right-arrow.svg);
    position: absolute;
    top: 5px;
    right: 7px;
  }
}
.emptyArray {
  height: 300px;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* animation */
.transition {
  margin: 0;
}
.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active,
.list-leave-active {
  transition: all 0.8s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}
</style>
