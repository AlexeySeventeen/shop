<template>
  <div class="pagination__wrapper" v-if="totalPage > 1">
    <button @click="paginationClick" class="pagination__element BACK">
      <svg
        fill="#000000"
        width="18px"
        height="18px"
        viewBox="0 0 24 24"
        xmlns="http://www.w3.org/2000/svg">
        <g data-name="Layer 2">
          <g data-name="arrow-ios-back">
            <path
              d="M13.83 19a1 1 0 0 1-.78-.37l-4.83-6a1 1 0 0 1 0-1.27l5-6a1 1 0 0 1 1.54 1.28L10.29 12l4.32 5.36a1 1 0 0 1-.78 1.64z" />
          </g>
        </g>
      </svg>
    </button>

    <button
      @click="paginationClick"
      class="pagination__element NUMBER"
      v-for="page in totalPage"
      :class="{active: page === currentPage}"
      :key="page">
      {{ page }}
    </button>

    <button @click="paginationClick" class="pagination__element FORWARD">
      Next
      <svg
        fill="#000000"
        width="18px"
        height="18px"
        viewBox="0 0 24 24"
        xmlns="http://www.w3.org/2000/svg">
        <g data-name="Layer 2">
          <g data-name="arrow-ios-forward">
            <path
              d="M10 19a1 1 0 0 1-.64-.23 1 1 0 0 1-.13-1.41L13.71 12 9.39 6.63a1 1 0 0 1 .15-1.41 1 1 0 0 1 1.46.15l4.83 6a1 1 0 0 1 0 1.27l-5 6A1 1 0 0 1 10 19z" />
          </g>
        </g>
      </svg>
    </button>
  </div>
</template>

<script>
export default {
  name: 'MyPagination',

  data() {
    return {
      currentPage: 1,
    };
  },

  props: {
    totalPage: {
      type: Number,
    },
    selectClick: Number,
  },

  methods: {
    paginationClick(a) {
      const classes = a.target.classList.value;
      // FORWARD
      if (classes.includes('FORWARD') && this.currentPage === this.totalPage) {
        this.currentPage = this.totalPage;
      } else if (classes.includes('FORWARD')) {
        this.currentPage++;
        this.scrollPage();
      }

      // BACK
      if (classes.includes('BACK') && this.currentPage === 1) {
        this.currentPage = 1;
      } else if (classes.includes('BACK')) {
        this.currentPage--;
        this.scrollPage();
      }

      // number
      if (classes.includes('NUMBER')) {
        this.currentPage = +a.target.innerHTML;
        this.scrollPage();
      }

      this.$emit('paginationClick', this.currentPage);
    },
    inputOrSelect() {
      this.currentPage = 1;
      this.$emit('paginationClick', this.currentPage);
    },

    scrollPage() {
      if (window.innerWidth > 740) {
        window.scrollTo({
          top: 600,
          behavior: 'smooth',
        });
      } else {
        window.scrollTo({
          top: 350,
          behavior: 'smooth',
        });
      }
    },
  },

  watch: {
    selectClick() {
      this.inputOrSelect();
    },
  },
};
</script>

<style scoped>
.pagination__wrapper {
  display: flex;
  justify-content: center;
  margin-top: 10px;
  &:hover {
    cursor: pointer;
  }
}
.pagination__element {
  border: 1px black solid;
  border-left: 0px;
  padding: 6px 10px;
  background-color: rgba(156, 147, 147, 0.233);
  display: flex;
  align-items: center;
}

.pagination__wrapper :first-child {
  border-left: 1px black solid;
  border-top-left-radius: 9px;
  border-bottom-left-radius: 9px;
}
.pagination__wrapper :last-child {
  border-left: 0px;
  border-top-right-radius: 9px;
  border-bottom-right-radius: 9px;
}

.pagination__element:hover {
  background-color: rgba(156, 147, 147, 52%);
}

.active {
  background-color: #92b2c1;
  &:hover {
    background-color: #92b2c1;
  }
}
svg {
  z-index: -1;
}
</style>
