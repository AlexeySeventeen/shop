<template>
  <div class="wrapper__main">
    <Header
      @openFavorite="openFavorite"
      @openCart="openBlock"
      @openProfile="openBlock"
      :forRound="cartArray.length" />

    <Alert v-if="alertActive" :message="messageAlert" />

    <!-- Cart -->
    <MyModal
      v-if="cartActive"
      :heading="'Cart Summary'"
      :backArrow="false"
      @close="closeBlock('cartActive')">
      <CartActive
        @checkoutFalse="showAlert('You need to log in to the site first')"
        @checkout="checkoutClick"
        :authorization="this.authorizationActive"
        :cartArray="cartArray"
        @counterChange="counterChange"
        @deleteFromCart="addToCart" />
    </MyModal>
    <!-- Checkout -->
    <MyModal
      v-if="checkoutActive"
      :heading="'Checkout'"
      :backArrow="true"
      @close="closeBlock('checkoutActive')"
      @backArrow="arrowBack('checkoutActive', 'cartActive')">
      <Checkout
        @checkoutComplete="checkoutComplete"
        :info="profileInfo"
        :newOrder="newOrder" />
    </MyModal>

    <!-- Favorite -->
    <div v-if="favoriteActive">
      <CardList
        @addToFavorite="addToFavorite"
        @addToCart="addToCart"
        :cards="favoriteArray"
        :numberOfCard="15" />
    </div>

    <!-- Profile -->
    <MyModal
      v-if="profileActive && !authorizationActive"
      :heading="'Authorization'"
      :backArrow="false"
      @close="closeBlock('profileActive')">
      <Profile
        @registration="arrowBack('profileActive', 'registrationActive')"
        @notAvailable="showAlert('This feature is not available now')"
        @completeAuthorization="completeAuthorization" />
    </MyModal>
    <MyModal
      v-if="profileActive && authorizationActive"
      :heading="'Profile'"
      :backArrow="false"
      @close="closeBlock('profileActive')">
      <AuthorizationProfile
        @saveComplete="showAlert('Your data has been successfully changed')"
        :info="profileInfo" />
    </MyModal>
    <!-- Registration -->
    <MyModal
      v-if="registrationActive"
      :heading="'Registration'"
      :backArrow="true"
      @close="closeBlock('registrationActive')"
      @backArrow="arrowBack('registrationActive', 'profileActive')">
      <Registration
        @notAvailable="showAlert('This feature is not available now')"
        @registrationComplete="registrationComplete" />
    </MyModal>

    <!-- default -->
    <div v-if="!favoriteActive">
      <Introduce />
      <div class="furniture__wrapper">
        <span class="furniture__text">Our Furniture</span>
        <MySelect
          @selectUpdate="selectUpdate"
          :categorySelect="Array.from(new Set(category))" />
      </div>
      <MyInput
        class="searchInput"
        :type="text"
        v-model="searchModel"
        :placeholder="'Search furniture..'" />
      <LoadingScreen v-if="isLoading" />
      <CardList
        v-else
        @cardClick="cardClick"
        @addToFavorite="addToFavorite"
        @addToCart="addToCart"
        :cards="sortedCardsAndSearchAndFavorite"
        :selectClick="selectClick"
        :numberOfCard="15" />

      <MyModal
        v-if="bigCardActive"
        :heading="bigCardName"
        :backArrow="false"
        @close="closeBlock('bigCardActive')">
        <BigCard
          class="wrapper__main"
          @addToFavorite="addToFavorite"
          @addToCart="addToCart"
          :card="bigCard" />
      </MyModal>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      cards: [],

      category: [],

      isLoading: false,

      favoriteActive: false,
      favoriteArray: JSON.parse(localStorage.getItem('favorite')) || [],

      cartActive: false,
      cartArray: JSON.parse(localStorage.getItem('cart')) || [],
      checkoutActive: false,

      bigCardActive: false,
      bigCardName: '',
      bigCard: '',

      alertActive: false,

      profileActive: false,
      profileInfo: false,
      authorizationActive: false,
      registrationActive: false,

      selectValue: '',
      searchModel: '',

      selectClick: 1,
      messageAlert: '',

      newOrder: {},
    };
  },

  methods: {
    counterChange(nameOfCard, sigh) {
      if (sigh === 'plus') {
        this.cartArray.forEach((cart) => {
          if (cart.name === nameOfCard) {
            cart.count++;
            localStorage.setItem('cart', JSON.stringify(this.cartArray));
            return;
          }
        });
      }
      if (sigh === 'minus') {
        this.cartArray.forEach((cart) => {
          if (cart.name === nameOfCard) {
            cart.count--;
            localStorage.setItem('cart', JSON.stringify(this.cartArray));
            return;
          }
        });
      }
    },

    checkoutClick(newOrderInfo) {
      this.arrowBack('cartActive', 'checkoutActive');
      this.newOrder = newOrderInfo;
    },

    checkoutComplete() {
      this.showAlert(
        'Operation has been completed successfully, you can view your order in the profile'
      );
      this.checkoutActive = false;
      this.cartArray = [];
      localStorage.setItem('cart', JSON.stringify(this.cartArray));
    },

    registrationComplete(obj) {
      this.profileInfo = obj;
      this.showAlert('You have successfully registered on the site');
      this.registrationActive = false;
      this.authorizationActive = true;
    },

    completeAuthorization(obj) {
      this.profileInfo = obj;
      this.showAlert('You have successfully logged in to your account');
      this.profileActive = false;
      this.authorizationActive = true;
    },

    showAlert(message) {
      this.messageAlert = message;
      this.alertActive = true;
      setTimeout(() => {
        this.alertActive = false;
        this.messageAlert = '';
      }, 2000);
    },

    selectUpdate(value) {
      this.selectValue = value;
      this.selectClick++;
    },

    cardClick(card) {
      this.bigCardName = card.name;
      this.bigCard = card;
      this.openBlock('bigCardActive');
    },

    openFavorite(a) {
      if (a.value === 'click') {
        this.favoriteActive = true;
      } else {
        this.favoriteActive = false;
      }
    },
    addToFavorite(cardName) {
      const arrayOfNames = [];
      this.favoriteArray.forEach((item) => {
        arrayOfNames.push(item.name);
      });
      if (arrayOfNames.includes(cardName.name)) {
        this.showAlert(`${cardName.name} has been removed from favorites`);
        const number = arrayOfNames.indexOf(cardName.name);
        this.favoriteArray.splice(number, 1);
        cardName.isFavorite = false;
        localStorage.setItem('favorite', JSON.stringify(this.favoriteArray));
      } else {
        this.showAlert(`${cardName.name} has been added to favorites`);
        this.favoriteArray.push(cardName);
        cardName.isFavorite = true;
        localStorage.setItem('favorite', JSON.stringify(this.favoriteArray));
      }
    },

    openBlock(what) {
      document.body.classList.add('stopScroll');
      this.$data[what] = true;
    },
    closeBlock(what) {
      document.body.classList.remove('stopScroll');
      this.$data[what] = false;
    },

    arrowBack(close, open) {
      this.closeBlock(close);
      this.openBlock(open);
    },

    addToCart(cardName) {
      const arrayOfNames = [];
      this.cartArray.forEach((item) => {
        arrayOfNames.push(item.name);
      });
      if (arrayOfNames.includes(cardName.name)) {
        const number = arrayOfNames.indexOf(cardName.name);
        this.showAlert(`${cardName.name} has been removed from the cart`);
        this.cartArray.splice(number, 1);
        cardName.isCart = false;
        localStorage.setItem('cart', JSON.stringify(this.cartArray));
      } else {
        this.showAlert(cardName.name, 'added to the cart');
        this.showAlert(`${cardName.name} has been added to the cart`);
        this.cartArray.push(cardName);
        cardName.isCart = true;
        localStorage.setItem('cart', JSON.stringify(this.cartArray));
      }
    },
  },

  computed: {
    sortedCards() {
      return [...this.cards].filter((card) => {
        if (this.selectValue !== '') {
          return card.category == this.selectValue;
        } else {
          return card;
        }
      });
    },
    sortedCardsAndSearch() {
      return this.sortedCards.filter((card) => {
        if (this.searchModel !== '') {
          this.selectClick++;
          return card.forSearch
            .toLowerCase()
            .includes(this.searchModel.toLowerCase());
        } else {
          return card;
        }
      });
    },
    sortedCardsAndSearchAndFavorite() {
      const arrayOfNames = [];
      this.favoriteArray.forEach((item) => {
        arrayOfNames.push(item.name);
      });
      return this.sortedCardsAndSearch.filter((card) => {
        if (arrayOfNames.includes(card.name)) {
          card.isFavorite = true;
          return card;
        } else {
          card.isFavorite = false;
          return card;
        }
      });
    },
  },

  mounted() {
    this.isLoading = true;
    setTimeout(() => {
      axios
        .get('https://56d336e4e8b8e4de.mokky.dev/furnitures')
        .then((response) => {
          this.cards = response.data;

          for (let index = 0; index < response.data.length; index++) {
            this.category.push(response.data[index].category);
          }

          this.isLoading = false;
        });
    }, 2000);
  },
};
</script>

<style>
/* pattern class */
* {
  font-family: 'Gilroy', sans-serif;
}
.wrapper__main {
  max-width: 1440px;
  margin: 0px auto;
}
button:hover {
  cursor: pointer;
}
.stopScroll {
  overflow: hidden;
}
body input.invalid {
  background-color: rgb(239 68 68);
}
.invalidText {
  margin-top: 5px;
  color: rgb(239 68 68);
  font-size: 12px;
}

/* hide scrollbar */
::-webkit-scrollbar {
  display: none;
  -ms-overflow-style: none;
  scrollbar-width: none;
}

/* App */
.furniture__wrapper {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin-top: 10px;
  margin-bottom: 4px;
  padding: 0px 10px;
}
.furniture__text {
  font-weight: 700;
  font-size: 28px;
  line-height: 120%;
  color: #3a3a3a;
}
.searchInput {
  position: relative;
  &:after {
    content: url(./components/all-svg/loupe.svg);
    position: absolute;
    top: 9px;
    right: 10px;
  }
}
</style>

<!-- how work with scss -->
<!-- local fonts in vue -->
