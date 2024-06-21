<template>
  <div class="wrapper__middle">
    <input
      :id="id"
      :value="modelValue"
      @input="InputUpdate"
      :type="type"
      :placeholder="placeholder"
      :maxlength="maxlength"
      :disabled="disabled" />
  </div>
</template>

<script>
export default {
  name: 'MyInput',

  data() {
    return {modelValue: ''};
  },
  props: {
    type: String,
    id: String,
    placeholder: String,
    maxlength: Number,
    modelValue: [String, Number],
    disabled: Boolean,
  },
  methods: {
    InputUpdate(event) {
      this.$emit('update:modelValue', event.target.value);
    },
  },
};
</script>

<style scoped>
.wrapper__middle {
  display: flex;
  position: relative;
}
input {
  font-weight: 400;
  font-size: 14px;
  line-height: 150%;
  color: #000;
  padding: 9px 30px 9px 10px;
  border: none;
  outline: 0px;
  background-color: rgba(156, 147, 147, 0.226);
  border-radius: 10px;
  width: 100%;
  box-shadow: 1px 3px 7px 0 rgba(0, 0, 0, 0.25);
  &:focus {
    outline: 2px #92b2c1 solid;
    box-shadow: 1px 3px 7px 0 #92b2c1;
  }
}
.loupe {
  position: absolute;
  top: 9px;
  right: 20px;
}
</style>
