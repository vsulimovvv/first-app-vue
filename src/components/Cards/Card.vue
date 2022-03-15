<template>
  <li class="card">
    <div v-if="type === 'post'">
      <h3 class="card__title">{{ card.title }}</h3>
      <p class="card__text">{{ card.body }}</p>

      <AppButton class="card__btn" buttonName="Удалить" @click="deletePost" />
    </div>
    <div v-if="type === 'user'">
      <p>username: {{ card.username }}</p>
      <p>email: {{ card.email }}</p>
      <p class="card__address">address: {{ address }}</p>
      <p>phone: {{ card.phone }}</p>
      <p>website: {{ card.website }}</p>
      <p>Company: {{ company }}</p>
    </div>
  </li>
</template>

<script>
import AppButton from "@/components/base/AppButton";
export default {
  name: "Card",
  components: {
    AppButton,
  },
  props: {
    type: {
      type: String,
      default: "post",
      validator: (value) => ["post", "user"].includes(value),
    },
    card: {
      type: Object,
      default: () => ({}),
    },
  },

  data() {
    return {};
  },

  computed: {
    address() {
      let result = "";
      let { city, street, suite, zipcode } = this.card.address;

      if (zipcode) {
        result += zipcode + " ";
      }
      if (city) {
        result += city + " ";
      }
      if (street) {
        result += street + " ";
      }
      if (suite) {
        result += suite;
      }

      return result;
    },
    company() {
      let result = "";
      let { name, catchPhrase } = this.card.company;
      if (name) {
        result += name + "/";
      }
      if (catchPhrase) {
        result += catchPhrase + "/";
      }
      return result;
    },
  },

  methods: {
    deletePost() {
      this.$emit("deletePost", this.card.id);
    },
  },
};
</script>

<style lang="scss">
.card {
  max-width: 300px;
  width: 100%;
  border: 1px solid #000;
  padding: 20px;

  justify-self: center;
  display: flex;
  flex-direction: column;
  align-items: flex-start;

  &__title {
    font-size: 24px;
  }

  &__text {
    font-size: 16px;
  }
  &__btn {
    margin-top: auto;
    background-color: #f00 !important;
  }
}
</style>
