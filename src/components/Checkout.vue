<template>
  <div>
    <h4>Card Number</h4>
    <MyInput
      id="number"
      :type="'text'"
      :placeholder="'1234 5678 9101 1121'"
      v-model="cardNumber"
      :maxlength="19" />
    <div class="invalidText" v-if="!cardNumberAccept">
      ∗this field must be filled in
    </div>

    <div class="wrapper">
      <div>
        <h4>Expiration Date</h4>
        <MyInput
          id="date"
          v-model="cardDate"
          :maxlength="5"
          :type="text"
          :placeholder="'MM/YY'" />
        <div class="invalidText" v-if="!dateAccept">
          ∗this field must be filled in
        </div>
      </div>
      <div>
        <h4>CVV</h4>
        <MyInput
          :type="text"
          :maxlength="3"
          :placeholder="'123'"
          v-model="cvv"
          id="cvv" />
        <div class="invalidText" v-if="!cvvAccept">
          ∗this field must be filled in
        </div>
      </div>
    </div>

    <h4>Email</h4>
    <MyInput v-model="email" type="email" :placeholder="'email@adrees.com'" />
    <div class="invalidText" v-if="!emailAccept">
      ∗this field must be filled in
    </div>

    <MyButton
      v-if="this.info.cardNumber.length === 0"
      @click="payComplete"
      class="button"
      >Pay now</MyButton
    >
    <MyButton
      v-if="this.info.cardNumber.length === 19"
      @click="payCompleteWithout"
      class="button"
      >Pay now</MyButton
    >

    <div class="questionBlock" v-if="saveCardInfoBlock">
      <h4>Would you like to save your card details?</h4>
      <MyButton @click="chooseBtn(true)">Yes</MyButton>
      <MyButton @click="chooseBtn(false)">No</MyButton>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Checkout',
  data() {
    return {
      cardNumber: this.info.cardNumber,
      cardDate: this.info.date,
      cvv: this.info.cvv,
      email: this.info.email,
      ordersArray: this.info.orders,
      saveCardInfoBlock: false,

      cardNumberAccept: true,
      emailAccept: true,
      dateAccept: true,
      cvvAccept: true,
    };
  },

  props: {
    info: Object,
    newOrder: Object,
  },

  watch: {
    cardNumber(numb) {
      document.querySelector('#number').addEventListener('keyup', function (e) {
        e.target.value = e.target.value.replace(/[^0-9 ]+/g, '');
      });
      if (numb.length === 4 || numb.length === 9 || numb.length === 14) {
        this.cardNumber = `${numb} `;
      }
    },

    cardDate(numb) {
      document.querySelector('#date').addEventListener('keyup', function (e) {
        e.target.value = e.target.value.replace(/[^0-9/]+/g, '');
      });
      if (numb.length === 2) {
        this.cardDate = `${numb}/`;
      }
    },

    cvv() {
      document.querySelector('#cvv').addEventListener('keyup', function (e) {
        e.target.value = e.target.value.replace(/[^0-9]+/g, '');
      });
    },
  },

  methods: {
    payComplete() {
      this.validation();

      if (
        this.cardNumberAccept === true &&
        this.emailAccept === true &&
        this.dateAccept === true &&
        this.cvvAccept === true
      ) {
        this.saveCardInfoBlock = true;
        this.ordersArray.push(this.newOrder);
      }
    },

    chooseBtn(what) {
      this.validation();

      if (
        this.cardNumberAccept === true &&
        this.emailAccept === true &&
        this.dateAccept === true &&
        this.cvvAccept === true
      ) {
        this.saveCardInfoBlock = false;

        if (what) {
          axios
            .patch(
              'https://56d336e4e8b8e4de.mokky.dev/profiles/' + this.info.id,
              {
                cardNumber: this.cardNumber,
                date: this.cardDate,
                cvv: this.cvv,
                orders: this.ordersArray,
              }
            )
            .then(() => {
              this.$emit('checkoutComplete');
            });
        } else {
          axios
            .patch(
              'https://56d336e4e8b8e4de.mokky.dev/profiles/' + this.info.id,
              {
                orders: this.ordersArray,
              }
            )
            .then(() => {
              this.$emit('checkoutComplete');
            });
        }
      }
    },

    payCompleteWithout() {
      this.validation();
      if (
        this.cardNumberAccept === true &&
        this.emailAccept === true &&
        this.dateAccept === true &&
        this.cvvAccept === true
      ) {
        this.ordersArray.push(this.newOrder);
        axios
          .patch(
            'https://56d336e4e8b8e4de.mokky.dev/profiles/' + this.info.id,
            {
              orders: this.ordersArray,
            }
          )
          .then(() => {
            this.$emit('checkoutComplete');
          });
      }
    },

    validation() {
      this.cardNumberAccept = true;
      this.emailAccept = true;
      this.dateAccept = true;
      this.cvvAccept = true;

      if (this.cardNumber.length !== 19) {
        this.cardNumberAccept = false;
      }
      if (this.email.length === 0) {
        this.emailAccept = false;
      }
      if (this.cvv.length !== 3) {
        this.cvvAccept = false;
      }
      if (this.cardDate.length !== 5) {
        this.dateAccept = false;
      }
    },
  },
};
</script>

<style scoped>
h4 {
  margin: 10px 0 5px;
}
.wrapper {
  display: flex;
  width: 340px;
  gap: 25px;
}
.button {
  background-color: #9ccce3;
  width: 100%;
  margin-top: 18px;
  &&:hover {
    background-color: #7cbad7;
  }
}
.questionBlock {
  position: absolute;
  top: calc(50% - 48px);
  left: 0;
  background-color: white;
  text-align: center;
  border-radius: 10px;
  padding: 10px 0;
  width: 100%;
}
</style>
