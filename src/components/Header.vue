<template>
  <div class="wrapper">
    <!-- left -->
    <Logo></Logo>

    <!-- right -->
    <div class="wrapper__right">
      <HeartBtn id="HeartBtn" @click="openFavorite" title="Favorite Products" />
      <div style="position: relative">
        <CartBtn id="CartBtn" @click="openCart" title="Shopping Cart" />
        <div class="round" :class="{none: forRound === 0}">
          {{ this.forRound }}
        </div>
      </div>
      <button @click="openProfile" class="svg" title="Profile">
        <svg
          height="28"
          viewBox="0 0 32 32"
          width="28"
          xmlns="http://www.w3.org/2000/svg">
          <path
            d="m16 8a5 5 0 1 0 5 5 5 5 0 0 0 -5-5zm0 8a3 3 0 1 1 3-3 3.0034 3.0034 0 0 1 -3 3z" />
          <path
            d="m16 2a14 14 0 1 0 14 14 14.0158 14.0158 0 0 0 -14-14zm-6 24.3765v-1.3765a3.0033 3.0033 0 0 1 3-3h6a3.0033 3.0033 0 0 1 3 3v1.3765a11.8989 11.8989 0 0 1 -12 0zm13.9925-1.4507a5.0016 5.0016 0 0 0 -4.9925-4.9258h-6a5.0016 5.0016 0 0 0 -4.9925 4.9258 12 12 0 1 1 15.985 0z" />
        </svg>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Header',

  props: {
    forRound: Number,
  },

  methods: {
    openFavorite() {
      const btn = document.querySelector('#HeartBtn');
      btn.classList.toggle('click');
      this.$emit('openFavorite', btn.classList);
    },
    openCart() {
      this.$emit('openCart', 'cartActive');
    },
    openProfile() {
      this.$emit('openProfile', 'profileActive');
    },
  },
  mounted() {
    this.round;
  },
};
</script>

<style scoped>
button {
  background-color: white;
  border: none;
  margin: 0;
  padding: 0;
}
.wrapper {
  margin-top: 5px;
  display: flex;
  align-items: center;
  flex-direction: row;
  justify-content: space-between;
  padding: 0px 10px;
  margin-bottom: 2px;
}
.wrapper__right {
  display: flex;
  align-items: center;
  gap: 10px;
}

.logo {
  margin-top: -6px;
}
.svg:hover svg path,
.svg:hover svg circle {
  fill: gray;
}

.round {
  position: absolute;
  top: -6px;
  right: -6px;
  background-color: #f58e4e;
  border-radius: 100%;
  width: 14px;
  height: 14px;
  text-align: center;
  font-size: 12px;
}
.none {
  display: none;
}
</style>
