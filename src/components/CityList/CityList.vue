<template>
  <div class="cities">
    <div class="cities__top">
      <h3 class="cities__title">Список городов</h3>
      <button class="cirites__icon">&#x271A;</button>
    </div>
    <div class="cities__undertop">
      <button class="">Выделить все</button>
      <button class="">Очистить</button>
    </div>

    <ul class="cities__list">
      <li class="cities__item" v-for="(city, index) in cityList" :key="index">
        <input class="cities__checkbox" type="checkbox" />
        <div class="cities__color" :style="getBackgroundColor(city.color)"></div>
        <h3 class="cities__heading">{{ city.name }}</h3>
        <span class="cities__area">{{ city.region }}</span>
        <button class="cities__delete">X</button>
      </li>
    </ul>
  </div>
</template>

<script>
// import Modal from "@/components/Modal/Modal";

export default {
  // ! Сделать отдельные компоненты

  name: "CityList",

  components: {
    // Modal,
  },

  props: {
    cityList: {
      type: Array,
      required: true,
      default: () => [],
    },
  },

  data() {
    return {};
  },

  methods: {
    getBackgroundColor(color) {
      return {
        backgroundColor: `#${color}`,
      };
    },
  },

  computed: {},
};
</script>

<style lang="scss" scoped>
.cities {
  max-width: 300px;
  color: #000;
  width: 100%;
  font-size: 16px;
  border: 1px solid #c4c4c4;
  border-radius: 5px;
  background-color: #fff;
  padding: 20px;
  margin: 0 auto;

  ul {
    list-style-type: none;
    padding: 0;
    margin: 0;
  }
  h1,
  h2,
  h3 {
    margin: 0;
  }

  &__top {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
  }

  &__list {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  &__item {
    padding: 10px;
    background-color: bisque;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 10px;
  }
  &__undertop {
    margin-bottom: 20px;

    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  &__color {
    width: 20px;
    height: 20px;
    background-color: #f00;
  }

  &__area {
    width: 30px;
    height: 30px;
    border: 1px solid #000;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 15px;
    color: #000;
  }
}
</style>
