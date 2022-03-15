<template>
  <button
  class="tabs__btn"
  @click="$emit('onClickTab', id)"
  :class="{ 'active': isActive }">
    <slot />
  </button>
</template>

<script>
export default {
  name: "TabsItem",
  props: {
    id: {
      type: Number,
      default: 0,
      required: true,
    },
    activeTab: {
      type: Number,
      default: 0,
      required: true,
    },
  },

  computed: {
    isActive() {
      if (this.id === this.activeTab) {
        return true;
      }
    },
  },
};
</script>

<style lang="scss">
.tabs__btn {
  background: #000;
  color: #fff;
  border: none;
  font-size: 20px;
  padding: 10px 30px;
  cursor: pointer;
  border-radius: 5px;

  &.active {
    background-color: bisque;
    color: #000;
  }
}
</style>
