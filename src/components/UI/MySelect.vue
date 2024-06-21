<template>
  <div>
    <div class="select__btn-wrapper">
      <button class="select__btn" @click="showOption">{{ selectValue }}</button>
      <svg
        fill="#000000"
        height="12px"
        width="12px"
        version="1.1"
        id="arrow-svg"
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        viewBox="0 0 330 330"
        xml:space="preserve">
        <path
          d="M325.607,79.393c-5.857-5.857-15.355-5.858-21.213,0.001l-139.39,139.393L25.607,79.393
	c-5.857-5.857-15.355-5.858-21.213,0.001c-5.858,5.858-5.858,15.355,0,21.213l150.004,150c2.813,2.813,6.628,4.393,10.606,4.393
	s7.794-1.581,10.606-4.394l149.996-150C331.465,94.749,331.465,85.251,325.607,79.393z" />
      </svg>
    </div>

    <div id="option" class="close">
      <!-- all products -->
      <button :key="all" @click="selectUpdate" :value="''">
        All products
        <div class="svg__select">
          <img src="../all-svg/svg-for-select/AllProducts.svg" />
        </div>
      </button>
      <!-- category -->
      <button
        v-for="button in categorySelect"
        :key="button"
        @click="selectUpdate"
        :value="button">
        {{ button }}
        <div class="svg__select">
          <img
            :src="require('../all-svg/svg-for-select/' + button + '.svg')"
            :alt="button" />
        </div>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MySelect',

  data() {
    return {
      selectValue: 'Choose category',
    };
  },

  props: {
    categorySelect: {
      type: Array,
      require: true,
    },
  },

  methods: {
    showOption() {
      const option = document.querySelector('#option');
      setTimeout(() => {
        option.classList.toggle('open');
      }, 200);

      const svg = document.querySelector('#arrow-svg');
      if (option.classList.contains('open')) {
        svg.setAttribute('transform', 'rotate(0)');
      } else {
        svg.setAttribute('transform', 'rotate(-180)');
      }
    },

    selectUpdate(e) {
      this.showOption();
      if (e.target.value === '') {
        this.selectValue = 'All products';
      } else {
        this.selectValue = e.target.value;
      }
      this.$emit('selectUpdate', e.target.value);
    },
  },
};
</script>

<style scoped>
.select__btn-wrapper {
  position: relative;
  cursor: pointer;
}
.select__btn {
  width: 150px;
  padding: 0px;
  font-size: 14px;
  background-color: white;
  border: 0;
  padding: 8px 22px 8px 8px;
  border-radius: 8px;
  border: 1px #00000070 solid;
}
svg {
  position: absolute;
  top: 11px;
  right: 6px;
  transition: 0.2s ease-in-out;
}
.open {
  position: absolute;
  z-index: 1;
  width: 148px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  background-color: white;
  border-radius: 10px;
  border: 1px #00000070 solid;
  margin-top: 2px;
}

.close button {
  display: none;
  position: relative;
}

.open button {
  display: flex;
  align-items: center;
  justify-content: space-between;
  border: 0;
  background-color: transparent;
  border-bottom: 1px #00000070 solid;
  padding: 8px 7px;
  font-size: 14px;
  &:first-child {
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
  }
  &:last-child {
    border: 0;
    border-bottom-left-radius: 10px;
    border-bottom-right-radius: 10px;
  }
  &:hover {
    background-color: rgb(236, 236, 236);
  }
}
.svg__select {
  position: absolute;
  top: 4px;
  right: 4px;
}
</style>
