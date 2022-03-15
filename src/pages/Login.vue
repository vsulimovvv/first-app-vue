<template>
  <div>
    <h1>Вход</h1>
    <form @submit.prevent>
      <input type="text" placeholder="Login" v-model="loginValue" />
      <input type="password" placeholder="Password" v-model="passwordValue" />
      <AppButton
        button-name="Вход"
        @onButtonClick="onSubmit"
        :disabled="disabled"
        :class="{ disabled: disabled }"
      />

      <p v-if="loginErrors">{{ loginErrors }}</p>
      <p v-if="passwordErrors">{{ passwordErrors }}</p>
    </form>
  </div>
</template>

<script>
import AppButton from "@/components/base/AppButton";
export default {
  name: "Login",
  components: {
    AppButton,
  },
  data() {
    return {
      loginValue: "user",
      loginErrors: "",
      passwordValue: "user",
      passwordErrors: "",
    };
  },
  watch: {
    loginValue() {
      this.validation("login", this.loginValue);
      console.log(123);
    },
    passwordValue() {
      this.validation("password", this.passwordValue);
      console.log(222);
    },
  },

  computed: {
    disabled() {
      if (this.loginValue && this.passwordValue) {
        return false;
      }
      return true;
    },
  },

  methods: {
    validation(name, value) {
      // console.log(name, value);
      if (name === "login") {
        if (!value.trim()) {
          this.loginErrors = "Пустой логин";
        }
        if (value.length < 4) {
          this.loginErrors = "Короткий логин";
        }
      }
      if (name === "password") {
        if (value.length < 8) {
          this.passwordErrors = "Короткий пароль";
        }
      }
    },

    onSubmit() {},
  },
};
</script>

<style lang="scss" scoped>
form {
  max-width: 400px;
  display: flex;
  flex-direction: column;
  gap: 10px;

  input {
    padding: 10px;
  }
}
.disabled {
  background: #f00;
}
</style>
