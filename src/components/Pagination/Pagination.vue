<template>
  <div class="pagination">
    <ul class="pagination__list">
      <li class="pagination__item" v-for="(item, idx) in paginationCalc" :key="idx">
        <button
          class="pagination__link"
          :class="{ 'pagination__link--active': currentPage === item }"
          type="button"
          @click="changePage(item)"
        >
          {{ item }}
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Pagination",
  props: {
    pagination: {
      type: Number, // * Кол-во элементов в массиве
      required: false,
    },

    currentPage: {
      type: Number,
      required: false,
    },

    amountCards: {
      type: Number,
      required: false,
    },
  },
  data() {
    return {};
  },

  methods: {
    changePage(el) {
      this.$emit("changePage", el);
    },
  },

  computed: {
    paginationCalc() {
      return Math.round(this.pagination / this.amountCards);
    },
  },
};
</script>

<style lang="scss" scoped>
.pagination {
  padding: 15px 0;

  @media screen and (max-width: 769px) {
    padding: 10px 0;
  }

  &__list {
    list-style: none;
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 0;
  }

  &__link {
    display: block;
    text-decoration: none;
    border: 1px solid #563d7c;
    background-color: #fff;
    color: #563d7c;
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 10px;
    border-radius: 5px;
    transition: 150ms ease, transform 0s;
    cursor: pointer;

    &:hover,
    &--active {
      background-color: #563d7c;
      border-color: transparent;
      color: #fff;
    }

    &:active {
      transform: scale(0.96);
    }
  }
}
</style>
