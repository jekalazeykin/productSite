<template>
<div v-show="showValidation" class="login">
  <form acrion="#" class="login-form">
    <div>
      <label class="login-form__label">Input login (email)</label>
      <input class="login-form__input" type="email" id="mail" name="mail" placeholder="your email" required v-model="email" @input="validateLogin()">
    </div>
    <div>
      <label class="login-form__label"> Pasword</label>
      <input class="login-form__input" id="userPassword" type="password" required @change="validateForm()">
    </div>
    <div>
      <label class="login-form__label"> Confirm pasword</label>
      <input class="login-form__input" id="confirmPassword" type="password" required @keyup="validateForm()">
    </div>
    <div class="login-form__button-container">
      <button class="login-form__button-container__btn" id="submit-btn" type="submit" name="action" @click="GoToProductPage()">Submit</button>
    </div>
  </form>
</div>
</template>

<script>
export default {
  name: 'ValidateForm',
  data() {
    return {
      email: '',
      langFile: [],
      showValidation: true
    }
  },
  methods: {
    validateLogin() {
      console.log(this.email);
      let email = document.getElementById("mail");
      if (email.validity.typeMismatch) {
        email.setCustomValidity('Input correct email adress');
      } else {
        email.setCustomValidity('');
        localStorage.setItem('email', email.value);
      }
    },
    validateForm() {

      let password = document.getElementById("userPassword"),
        confirmPassword = document.getElementById("confirmPassword");

      if (password.value != confirmPassword.value) {
        confirmPassword.setCustomValidity("Passwords Don't Match");
      } else {
        confirmPassword.setCustomValidity('');
        localStorage.setItem('password', password.value);
      }
    },
    GoToProductPage() {

      if (!!localStorage.getItem('email') && !!localStorage.getItem('password')) {
        this.showValidation = false;
      }

    },

  },
  created: function() {
    this.GoToProductPage();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
body {
    font-family: Roboto, sans-serif;
}
.login {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 100vh;
    background-image: url("../assets/background.jpg");
    background-size: cover;
    background-repeat: no-repeat;
}
.login-form {
    padding: 10px 20px;
    box-shadow: 0 0 2px 2px rgba(#414141, 0.3);
    background: linear-gradient(to bottom, #6799ea, #abb9d6);
    font-size: 18px;
    border-radius: 10px;
    > div {
        padding: 4px 0;
    }
    &__label {
        display: inline-block;
        width: 170px;
        padding-right: 10px;
        text-align: right;
        vertical-align: middle;
    }
    &__input {
        width: 150px;
        height: 25px;
        line-height: 18;
        padding: 0 10px;
        outline: 0;
        border: 0;
        color: white;
        border: 1px solid #939496;
        vertical-align: middle;
        color: #272623;
        border-radius: 5px;
        background-color: #fff;
    }
    &__button-container {
        display: flex;
        justify-content: center;
        &__btn {
            padding: 3px;
        }
    }

}
</style>
