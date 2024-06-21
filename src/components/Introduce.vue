<template>
  <div class="introduce__wrapper">
    <!-- text -->
    <div class="introduce__block">
      <h1 class="introduce__boldText">Perfect Harmony: Comfort & Style</h1>
      <p class="introduce__regularText">
        Our furniture is made from selected and best quality materials that are
        suitable for your dream home.
      </p>
      <p class="introduce__regularText">
        Explore furniture that harmoniously combines comfort and style to
        elevate your home.
      </p>
      <button class="btn" @click="scrollPage">Shop now</button>
    </div>

    <!-- img -->
    <div class="img__wrapper">
      <img src="../../img/Introduce photo.png" alt="intro" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Introduce',
  methods: {
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
};
</script>

<style scoped>
.introduce__wrapper {
  background: #92b2c1;
  border-radius: 10px;
  height: 554px;
  display: grid;
  grid-template-columns: 1fr 475px;
  justify-items: center;
}
.introduce__block {
  max-width: 510px;
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.introduce__boldText {
  font-weight: 700;
  font-size: 60px;
  line-height: 120%;
  color: #000000e6;
  margin: 0px 0px;
  margin-bottom: 30px;
}
.introduce__regularText {
  font-weight: 500;
  font-size: 20px;
  line-height: 150%;
  color: #161515;
  margin: 0px 0px;
}

img {
  border-radius: 10px;
  object-fit: cover;
  max-height: 554px;
}

.btn {
  background-color: #f58e4e;
  border-radius: 14px;
  padding: 14px 39px;
  border: none;
  color: #fff;
  font-weight: 600;
  font-size: 18px;
  line-height: 150%;
  margin-top: 55px;
  &&:hover {
    background-color: #f89c63;
  }
}

@media (max-width: 960px) {
  .introduce__wrapper {
    grid-template-columns: 1fr;
    height: auto;
    padding: 20px 10px;
  }
  .introduce__boldText {
    font-size: 44px;
    margin-bottom: 15px;
  }
  .introduce__regularText {
    font-size: 14px;
  }
  .btn {
    margin-top: 15px;
    padding: 10px 30px;
    font-size: 14px;
  }
  img {
    display: none;
  }
}
</style>
