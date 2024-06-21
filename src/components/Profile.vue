<template>
  <div>
    <!-- Email -->
    <h4>Email</h4>
    <MyInput
      :id="'email'"
      v-model="email"
      type="email"
      :placeholder="'email@adrees.com'" />
    <div class="invalidText" v-if="emailInvalid">
      ∗email address was entered incorrectly
    </div>
    <!-- Password -->
    <h4>Password</h4>
    <MyInputPassword :id="'password'" v-model="password" />
    <div class="invalidText" v-if="passwordInvalid">
      ∗password was entered incorrectly
    </div>
    <!-- Registration -->
    <div class="registration__wrapper">
      <MyButton class="loginBtn" @click="login">Login</MyButton>
      <button class="link" @click="this.$emit('registration', false)">
        <p class="loginText" @click="this.$emit('registration', false)">
          Registration
        </p>
      </button>
    </div>
    <SignWith @notAvailable="notAvailable" :sign="'in'" />
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Profile',

  data() {
    return {
      email: '',
      password: '',
      approvedLogin: false,
      approvedPassword: false,
      emailInvalid: false,
      passwordInvalid: false,
      profileInfo: null,
    };
  },

  methods: {
    notAvailable(text) {
      this.$emit('notAvailable', text);
    },
    login() {
      axios
        .get('https://56d336e4e8b8e4de.mokky.dev/profiles')
        .then((response) => {
          this.emailInvalid = false;
          this.passwordInvalid = false;
          this.approvedPassword = false;
          this.approvedLogin = false;
          response.data.forEach((item) => {
            if (item.email === this.email) {
              this.approvedLogin = true;
            }
            if (item.password === this.password) {
              this.approvedPassword = true;
            }
            if (item.email === this.email && item.password === this.password) {
              this.profileInfo = item;
              return;
            }
          });
        })
        .then(() => {
          if (this.approvedLogin === false && this.approvedPassword === false) {
            document.querySelector('#email').classList.add('invalid');
            document.querySelector('#password').classList.add('invalid');
            this.emailInvalid = true;
            this.passwordInvalid = true;
            return;
          }
          if (this.approvedLogin === false) {
            document.querySelector('#email').classList.add('invalid');
            document.querySelector('#password').classList.remove('invalid');
            this.emailInvalid = true;
            return;
          }
          if (this.approvedPassword === false) {
            document.querySelector('#password').classList.add('invalid');
            document.querySelector('#email').classList.remove('invalid');
            this.passwordInvalid = true;
            return;
          }
          if (this.approvedLogin === true && this.approvedPassword === true) {
            this.$emit('completeAuthorization', this.profileInfo);
          }
        });
    },
  },
};
</script>

<style scoped>
h4 {
  margin: 10px 0 5px;
}

.registration__wrapper {
  margin-top: 15px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.loginText {
  cursor: pointer;
  font-size: 12px;
  font-weight: 600;
  /* margin-bottom: 8px; */
  color: #5194b3;
  &:hover {
    color: #6abde3;
  }
}
.loginBtn {
  background-color: #9ccce3;
  &&:hover {
    background-color: #7cbad7;
  }
}

.link {
  border: 0;
  background: transparent;
  border-radius: 10px;
  margin: 0;
}
</style>
