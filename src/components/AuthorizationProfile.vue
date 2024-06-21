<template>
  <div class="wrapper">
    <!-- Profile -->
    <h4>Profile info</h4>
    <div class="block__wrapper">
      <p>Full name:</p>
      <MyInput
        :placeholder="'no information here yet'"
        :disabled="true"
        v-model="name"
        id="change1" />
      <button @click="ableInput" class="block__btn">
        <img src="./all-svg/pencil.svg" title="Change info" alt="change1" />
      </button>
    </div>
    <div class="block__wrapper">
      <p>Email:</p>
      <MyInput
        :placeholder="'no information here yet'"
        :disabled="true"
        v-model="email"
        id="change2" />
      <button @click="ableInput" class="block__btn">
        <img src="./all-svg/pencil.svg" title="Change info" alt="change2" />
      </button>
    </div>
    <div class="block__wrapper">
      <p>Password:</p>
      <MyInput
        :placeholder="'no information here yet'"
        :disabled="true"
        v-model="password"
        id="change3" />
      <button @click="ableInput" class="block__btn">
        <img src="./all-svg/pencil.svg" title="Change info" alt="change3" />
      </button>
    </div>
    <!-- Checkout -->
    <h4>Payment info</h4>
    <div class="block__wrapper">
      <p>Card Number:</p>
      <MyInput
        :placeholder="'no information here yet'"
        :disabled="true"
        v-model="cardNumber"
        id="change4" />
      <button @click="ableInput" class="block__btn">
        <img src="./all-svg/pencil.svg" title="Change info" alt="change4" />
      </button>
    </div>
    <div class="block__wrapper">
      <p>Expiration date:</p>
      <MyInput
        :placeholder="'no information here yet'"
        :disabled="true"
        v-model="date"
        id="change5" />
      <button @click="ableInput" class="block__btn">
        <img src="./all-svg/pencil.svg" title="Change info" alt="change5" />
      </button>
    </div>
    <div class="block__wrapper">
      <p>CVV:</p>
      <MyInput
        :placeholder="'no information here yet'"
        :disabled="true"
        v-model="cvv"
        id="change6" />
      <button @click="ableInput" class="block__btn">
        <img src="./all-svg/pencil.svg" title="Change info" alt="change6" />
      </button>
    </div>

    <MyButton @click="save" class="save__btn">Save changes</MyButton>
    <!-- history -->
    <h4>Order history</h4>
    <div v-if="this.info.orders.length === 0">
      <h4 class="noProducts">You haven't bought any products yet..</h4>
    </div>
    <div v-else>
      <div class="order__wrapper" v-for="orderInfo in this.info.orders">
        <p class="order__heading">Order from {{ orderInfo.date }}</p>
        <div class="product__wrapper" v-for="order in orderInfo.products">
          <img class="img" :src="order.picture" :alt="order.name" />
          <div class="product__text">
            <p>{{ order.name }}</p>
            <p class="product__inscription">{{ order.inscription }}</p>
          </div>
          <div style="text-align: center">
            <p>{{ order.price }}€</p>
            <p class="product__inscription">count: {{ order.count }}</p>
          </div>
        </div>
        <p class="order__footer">Total price:{{ orderInfo.totalPrice }}€</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'AuthorizationProfile',
  data() {
    return {
      name: this.info.fullName,
      email: this.info.email,
      password: this.info.password,
      cardNumber: this.info.cardNumber,
      date: this.info.date,
      cvv: this.info.cvv,
    };
  },
  props: {
    info: Object,
  },
  methods: {
    ableInput(input) {
      document.querySelectorAll('input').forEach((input) => {
        input.disabled = true;
      });
      const currentInput = document.querySelector('#' + input.target.alt);
      currentInput.disabled = false;
      currentInput.focus();
    },

    save() {
      axios
        .patch('https://56d336e4e8b8e4de.mokky.dev/profiles/' + this.info.id, {
          email: this.email,
          password: this.password,
          fullName: this.name,
          cardNumber: this.cardNumber,
          date: this.date,
          cvv: this.cvv,
        })
        .then(() => {
          this.$emit('saveComplete');
        });
    },
  },
};
</script>

<style scoped>
.wrapper {
  max-height: 300px;
  padding-right: 10px;
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
h4 {
  margin: 0;
  margin-top: 10px;
  font-size: 20px;
}
.block__wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 10px;
}
p {
  font-size: 14px;
  margin: 0;
}

.order__wrapper {
  margin: 16px 0;
  & p {
    font-size: 16px;
    font-weight: 600;
  }
}
.order__heading {
  text-align: center;
}
.order__footer {
  text-align: end;
}
.product__wrapper {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 2px 0;
}
.img {
  width: 50px;
  height: 50px;
}
.product__text {
  text-align: center;
}
.product__inscription {
  font-size: 12px !important;
  color: #898989;
}
.block__wrapper {
  position: relative;
}
.block__btn {
  position: absolute;
  width: 0;
  height: 0;
  top: 9px;
  right: 20px;
  border: 0;
  background-color: transparent;
}
.save__btn {
  width: 100%;
  margin: 15px 0 10px;
  background-color: #9ccce3;
  &&:hover {
    background-color: #7cbad7;
  }
}
.noProducts {
  text-align: center;
  font-size: 16px;
  margin: 20px 0;
}
</style>
