<template>
  <input
    type="text"
    placeholder="Введите название"
    v-model="localValue"
    @update:modelValue="$emit('change', $event)"
  />
</template>

<script>
export default {
  name: "AppInput",
  props: {
    modelValue: {
      type: String,
    },
  },
  data() {
    return {
      localValue: "",
    };
  },
  emits: ["change"],
};
</script>

<style lang="scss" scoped>
input {
  padding: 10px;
  background-color: #f0f0f0;
  color: #000;
  max-width: 300px;
  margin: 0 auto;
  display: block;
  margin-top: 20px;
  margin-bottom: 10px;
  outline: none;
  border-radius: 10px;
}
</style>
