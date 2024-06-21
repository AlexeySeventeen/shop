<template>
  <button>
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'MyButton',
};
</script>

<style scoped>
button {
  background-color: rgba(156, 147, 147, 0.226);
  border-radius: 14px;
  padding: 12px 24px;
  border: none;
  color: black;
  font-weight: 600;
  font-size: 14px;
  &&:hover {
    background-color: rgba(156, 147, 147, 52%);
  }
}
</style>
