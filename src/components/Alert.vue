<template>
  <div>
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  name: 'Alert',
  props: {
    message: String,
  },
};
</script>

<style scoped>
div {
  position: sticky;
  top: 0;
  right: 0;
  z-index: 99;
  width: 100%;
  height: 0;
  display: flex;
  justify-content: flex-end;
}
p {
  margin: 0;
  padding: 0;
  width: 200px;
  height: 50px;
  font-size: 14px;
  background-color: white;
  border-radius: 8px;
  border: 1px black solid;
  padding: 2px 5px;
  margin-top: 20px;
  margin-right: 10px;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
</style>
