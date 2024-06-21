<template>
  <div class="modal__wrapper" @click="this.$emit('close', false)">
    <div class="modal__block scale-up-center" @click.stop>
      <div class="modal__close">
        <button
          title="Go Back"
          @click="this.$emit('backArrow', false)"
          v-if="backArrow"
          class="backArrow"></button>

        <p class="modal__text">{{ heading }}</p>

        <button @click="this.$emit('close', false)" title="Close Window">
          <svg
            viewBox="0 0 48 48"
            height="24"
            width="24"
            xmlns="http://www.w3.org/2000/svg">
            <path
              d="M38 12.83l-2.83-2.83-11.17 11.17-11.17-11.17-2.83 2.83 11.17 11.17-11.17 11.17 2.83 2.83 11.17-11.17 11.17 11.17 2.83-2.83-11.17-11.17z" />
            <path d="M0 0h48v48h-48z" fill="none" />
          </svg>
        </button>
      </div>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MyModal',
  props: {
    heading: String,
    backArrow: Boolean,
  },
};
</script>

<style scoped>
.modal__wrapper {
  position: fixed;
  background-color: rgba(0, 0, 0, 0.4);
  z-index: 10;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal__block {
  border-radius: 10px;
  background-color: #fff;
  min-width: 340px;
  padding: 10px;
  margin: 10px;
}
.modal__close {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.modal__text {
  font-weight: 700;
  font-size: 28px;
  padding: 0;
  margin: 0;
}
button {
  background-color: #fff;
  border: 0;
  padding-right: 0;
  padding: 0;
}
svg:hover {
  fill: rgb(158, 34, 21);
}

.backArrow {
  position: relative;
  width: 24px;
  height: 25px;
  &:before {
    content: url(../all-svg/left-arrow.svg);
    position: absolute;
    top: -2px;
    left: 0;
  }
  &:hover::before {
    content: url(../all-svg/left-arrow-hover.svg);
    position: absolute;
    top: -1px;
    left: 0;
  }
}

.scale-up-center {
  -webkit-animation: scale-up-center 0.4s cubic-bezier(0.39, 0.575, 0.565, 1)
    both;
  animation: scale-up-center 0.4s cubic-bezier(0.39, 0.575, 0.565, 1) both;
}
@-webkit-keyframes scale-up-center {
  0% {
    -webkit-transform: scale(0.5);
    transform: scale(0.5);
  }
  100% {
    -webkit-transform: scale(1);
    transform: scale(1);
  }
}
@keyframes scale-up-center {
  0% {
    -webkit-transform: scale(0.5);
    transform: scale(0.5);
  }
  100% {
    -webkit-transform: scale(1);
    transform: scale(1);
  }
}

@media (max-width: 500px) {
  .modal__block {
    margin-bottom: 20%;
  }
}
</style>
