<template>
  <div>
    <button @click="update">Update</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'OrderCard',

  methods: {
    update() {
      axios.patch('https://56d336e4e8b8e4de.mokky.dev/profiles/2', {
        id: 3,
        orders: 123,
      });
    },
  },
};
</script>

<style scoped></style>
