<template>
  <div class="tabs">
    <tabs-item
      v-for="(tab, index) in tabs"
      :key="index"
      :id="index"
      @onClickTab="clickTab"
      :activeTab="activeTab"
    >
      {{ tab.name }}
    </tabs-item>
  </div>
</template>

<script>
import TabsItem from "@/components/Tabs/TabsItem";
export default {
  name: "Tabs",
  components: {
    TabsItem,
  },
  props: {
    tabs: {
      type: Array,
      default: () => [],
      required: true,
    },
  },

  data() {
    return {
      activeTab: 0,
    };
  },

  methods: {
    clickTab(id = 0) {
      this.activeTab = id;
      this.$emit("onClickTab",id);
    },
  },
};
</script>

<style lang="scss">
.tabs {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
}
</style>
