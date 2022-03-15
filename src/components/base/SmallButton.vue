<template>
  <button class="btn">
    <slot />
  </button>
</template>

<script>
export default {
  name: "SmallButton",
  components: {},
};
</script>

<style lang="scss" scoped>
.btn {
  font-family: Arial, Helvetica, sans-serif;
  color: #fff;
  background-color: #f00;
  font-size: 16px;
  padding: 10px 20px;
  cursor: pointer;
  border: none;
  outline: none;
}
</style>
