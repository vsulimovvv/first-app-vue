<template>
  <!-- <MainPage /> -->
  <!-- <Login /> -->
  <!-- <Posts /> -->
  <CryptocurrencyPage />
</template>

<script>
import MainPage from "./pages/MainPage";
import Login from "./pages/Login";
import Posts from "./pages/Posts";
import CryptocurrencyPage from "./pages/CryptocurrencyPage";

export default {
  name: "App",
  components: { MainPage, Login, Posts, CryptocurrencyPage },
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
  font-family: inherit; /* 1 */
  font-size: 100%; /* 1 */
  line-height: 1.15; /* 1 */
  margin: 0; /* 2 */
}
</style>
