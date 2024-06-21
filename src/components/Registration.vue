<template>
  <div>
    <form @submit.prevent>
      <!-- full name -->
      <h4>Full name</h4>
      <MyInput v-model="fullName" type="text" :placeholder="'John Doe'" />
      <div class="invalidText" v-if="!filledFullName">
        ∗this field is not filled in
      </div>
      <!-- email -->
      <h4>Email</h4>
      <MyInput v-model="email" type="email" :placeholder="'email@adrees.com'" />
      <div class="invalidText" v-if="!emailValid">
        ∗email address was entered incorrectly
      </div>
      <!-- Password -->
      <h4>Password</h4>
      <MyInputPassword v-model="password" />
      <div class="invalidText" v-if="!passwordBigLetter">
        ∗password must contain at least two uppercase English letters
      </div>
      <div class="invalidText" v-if="!passwordLength">
        ∗password must contain at least 5 characters
      </div>
      <div class="invalidText" v-if="!passwordNumber">
        ∗password must contain at least one number
      </div>
      <!-- Repeat password -->
      <h4>Repeat password</h4>
      <MyInputPassword v-model="repeatPassword" />
      <div class="invalidText" v-if="!passwordsIdentical">
        ∗passwords do not match
      </div>

      <MyButton @click="registration" type="submit" class="registrationBtn">
        Registration
      </MyButton>
    </form>

    <SignWith @notAvailable="notAvailable" :sign="'up'" />
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Registration',

  data() {
    return {
      email: '',
      fullName: '',
      password: '',
      repeatPassword: '',

      passwordsIdentical: true,
      passwordLength: true,
      passwordBigLetter: true,
      passwordNumber: true,

      filledFullName: true,

      emailValid: true,

      numbers: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'],
      letters: [
        'A',
        'B',
        'C',
        'D',
        'E',
        'F',
        'G',
        'H',
        'I',
        'J',
        'K',
        'L',
        'M',
        'N',
        'O',
        'P',
        'Q',
        'R',
        'S',
        'T',
        'U',
        'V',
        'W',
        'X',
        'Y',
        'Z',
      ],
    };
  },

  methods: {
    notAvailable(text) {
      this.$emit('notAvailable', text);
    },

    registration() {
      this.validationPassword();
      this.filledFullNameFn();
      this.emailCheck();

      if (
        this.passwordsIdentical === true &&
        this.passwordLength === true &&
        this.passwordBigLetter === true &&
        this.passwordNumber === true &&
        this.filledFullName === true &&
        this.emailValid === true
      ) {
        axios
          .post('https://56d336e4e8b8e4de.mokky.dev/profiles', {
            email: this.email,
            password: this.password,
            fullName: this.fullName,
            cardNumber: '',
            date: '',
            cvv: '',
            orders: [],
          })
          .then(() => {
            axios
              .get('https://56d336e4e8b8e4de.mokky.dev/profiles')
              .then((response) => {
                const lastAccount = response.data.pop();
                this.$emit('registrationComplete', lastAccount);
              });
          });
      }
    },

    validationPassword() {
      this.passwordsIdentical = true;
      this.passwordLength = true;
      this.passwordNumber = true;
      this.passwordBigLetter = true;

      const arrayPassword = this.password.split('');
      const numb = [];
      const letter = [];

      if (this.repeatPassword !== this.password) {
        this.passwordsIdentical = false;
      }
      if (this.password.length < 5) {
        this.passwordLength = false;
      }

      arrayPassword.forEach((symbol) => {
        if (this.numbers.includes(symbol)) {
          numb.push(symbol);
        }
        if (this.letters.includes(symbol)) {
          letter.push(symbol);
        }
      });

      if (numb.length === 0) {
        this.passwordNumber = false;
      }
      if (letter.length < 1) {
        this.passwordBigLetter = false;
      }
    },

    filledFullNameFn() {
      this.filledFullName = true;

      if (this.fullName.length === 0) {
        this.filledFullName = false;
      }
    },

    emailCheck() {
      this.emailValid = true;

      const emailArray = this.email.split('');

      if (!emailArray.includes('@')) {
        this.emailValid = false;
        return;
      }

      if (emailArray.slice(0, emailArray.indexOf('@')).length === 0) {
        this.emailValid = false;
        return;
      }

      if (
        emailArray[emailArray.length - 3] === '.' ||
        emailArray[emailArray.length - 4] === '.'
      ) {
        this.emailValid = true;
      } else {
        this.emailValid = false;
      }
    },
  },
};
</script>

<style scoped>
h4 {
  margin: 10px 0 5px;
}
.registrationBtn {
  background-color: #9ccce3;
  width: 100%;
  margin-top: 18px;
  &&:hover {
    background-color: #7cbad7;
  }
}
</style>
