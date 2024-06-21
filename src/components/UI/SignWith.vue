<template>
  <div>
    <!-- Sign in block -->
    <div class="signBlock">
      <p>Sign {{ sign }} with</p>
    </div>
    <!-- Sign in buttons -->
    <div class="sign__wrapper">
      <MyButton @click="warnDisabled" class="google">Google</MyButton>
      <MyButton @click="warnDisabled" class="github">GitHub</MyButton>
      <MyButton @click="warnDisabled" class="facebook">Facebook</MyButton>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SignWith',
  data() {
    return {
      disabled: false,
    };
  },
  props: {
    sign: String,
  },
  methods: {
    warnDisabled(btn) {
      this.$emit("notAvailable", false);
      this.disabled = true;
      btn.target.classList.add('shake');
      setTimeout(() => {
        this.disabled = false;
        btn.target.classList.remove('shake');
      }, 1500);
    },
  },
};
</script>

<style scoped>
.signBlock {
  position: relative;
  margin: 20px;
  height: 1px;
  border-bottom: 1px rgba(156, 147, 147) solid;
  font-weight: 600;
  font-size: 16px;
  & p {
    position: absolute;
    color: rgba(156, 147, 147);
    background-color: white;
    padding: 0px 2px;
    margin: 0;
    top: -9px;
    left: calc(50% - 47px);
  }
}

.sign__wrapper {
  display: flex;
  justify-content: space-between;
  & button {
    padding-left: 38px;
    padding-right: 18px;
    position: relative;
  }
}
.google:before {
  content: url(../all-svg/social-icon/google.svg);
  position: absolute;
  top: 8px;
  left: 8px;
}
.github:before {
  content: url(../all-svg/social-icon/github.svg);
  position: absolute;
  top: 8px;
  left: 8px;
}
.facebook:before {
  content: url(../all-svg/social-icon/facebook.svg);
  position: absolute;
  top: 5.4px;
  left: 7px;
}

.shake {
  animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
  transform: translate3d(0, 0, 0);
}

@keyframes shake {
  10%,
  90% {
    transform: translate3d(-1px, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(2px, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-4px, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(4px, 0, 0);
  }
}
</style>
