<template>
  <!-- <MainPage /> -->
  <!-- <Login /> -->
  <!-- <Posts /> -->
  <!-- <CryptocurrencyPage /> -->
  <ModalPage />
</template>

<script>
import MainPage from "./pages/MainPage";
import Login from "./pages/Login";
import Posts from "./pages/Posts";
import CryptocurrencyPage from "./pages/CryptocurrencyPage";
import ModalPage from "./pages/ModalPage";

export default {
  name: "App",
  components: { MainPage, Login, Posts, CryptocurrencyPage, ModalPage },
};
</script>

<style lang="scss">
html {
  box-sizing: border-box;
}
*,
*::after,
*::before {
  box-sizing: inherit;
}
body {
  background-color: #f1f1f1;
  font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
}
button {
  margin: 0;
  padding: 0;
  cursor: pointer;
  font-family: inherit;
  font-size: 100%;
  line-height: 1.15;
  margin: 0;
  background: transparent;
  border: none;
}
</style>
