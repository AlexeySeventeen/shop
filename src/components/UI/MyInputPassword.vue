<template>
  <div class="password__wrapper">
    <MyInput
      :id="id"
      :value="modelValue"
      @input="InputUpdate"
      id="password"
      class="password"
      type="password"
      :placeholder="'•••••••••'" />
    <button @click="showPassword" id="btnEye">
      <img
        title="Show Password"
        class="open-eye"
        src="../all-svg/eye-open.svg"
        alt="show password" />
      <img
        title="Hide Password"
        class="close-eye"
        src="../all-svg/eye-close.svg"
        alt="show password" />
    </button>
  </div>
</template>

<script>
export default {
  name: 'MyInputPassword',

  data() {
    return {modelValue: ''};
  },

  props: {
    id: String,
  },

  methods: {
    showPassword(img) {
      const btn = document.querySelector('#btnEye');
      const input = btn.offsetParent.querySelector('input');
      btn.classList.toggle('open');
      if (btn.classList.value === 'open') {
        input.setAttribute('type', 'text');
      } else {
        input.setAttribute('type', 'password');
      }
      input.focus();
    },
    InputUpdate(event) {
      this.$emit('update:modelValue', event.target.value);
    },
  },
};
</script>

<style scoped>
.password__wrapper {
  position: relative;

  & button {
    width: 0;
    height: 0;
    padding: 0;
    position: absolute;
    top: 12px;
    right: 26px;
    border: 0;
    background-color: transparent;
    &.open {
      top: 8px;
      right: 28px;
    }
    &.open .open-eye {
      display: none;
    }
    &.open .close-eye {
      display: inline-block;
    }
  }
}
.close-eye {
  display: none;
}
</style>
