<template>
  <ul>
    <Card
      v-for="(card, index) in list"
      :key="index"
      :type="typeName"
      :card="card"
      @deletePost="deletePost"
    />
  </ul>
</template>

<script>
import Card from "@/components/Cards/Card";

export default {
  name: "CardsList",
  components: {
    Card,
  },
  props: {
    list: {
      type: Array,
      default: () => [],
      requiered: true,
    },
    typeName: {
      type: String,
      default: "post",
      validator: (value) => ["post", "user"].includes(value),
    },
  },

  data() {
    return {};
  },

  computed: {},

  methods: {
    deletePost(postId) {
      this.$emit("deletePost", postId);
    },
  },
};
</script>

<style lang="scss">
ul {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
}
</style>
