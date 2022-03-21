<template>
  <button class="btn" @click="$emit('onButtonClick')">{{ buttonName }}</button>
</template>

<script>
export default {
  name: "AppButton",
  props: {
    buttonName: {
      type: String,
      default: "",
      required: true,
    },
  },

  emits: ["onButtonClick"],
};
</script>

<style scoped>
.btn {
  color: #fff;
  background-color: rgb(33, 73, 185);
  font-size: 16px;
  padding: 10px 20px;
  cursor: pointer;
  border: none;
  outline: none;
}
</style>
