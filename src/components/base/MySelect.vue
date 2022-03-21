<template>
  <select class="select" @change="onChange">
    <option v-for="(item, index) of myOptions" :key="index" :value="item.value">
      {{ item.name }}
    </option>
  </select>
</template>

<script>
export default {
  name: "MySelect",
  props: {
    optionsList: {
      type: Array,
      default: () => [],
      required: true,
    },
  },

  emits: ["onChange"],

  data() {
    return {};
  },

  methods: {
    onChange(event) {
      this.$emit("onChange", event.target.value);
    },
  },
  computed: {
    myOptions() {
      return this.optionsList;
    },
  },
};
</script>

<style>
.select {
  height: 40px;
  padding: 10px;
  cursor: pointer;
}
</style>
