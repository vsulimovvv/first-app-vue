<template>
  <BaseModal @closeModal="closeModal">
    <template v-slot:title>Список городов</template>

    <template v-slot:content>
      <template v-if="modalType === 'default'">
        <select class="popup__select" @change="currentValue($event.target.value)">
          <option value="-1">Выберите значение</option>
          <option :value="index" v-for="(city, index) in cityList" :key="index">
            {{ city.name }}
          </option>
        </select>

        <div class="popup__btns">
          <button class="popup__btn popup__btn--cancel" @click="closeModal" type="button">
            Отмена
          </button>
          <button class="popup__btn" @click="addData" type="button" :disabled="isDisabled">
            Добавить
          </button>
        </div>
      </template>

      <template v-if="modalType === 'load'">
        <p>Загрузка</p>
      </template>

      <template v-if="modalType === 'success'">
        <p>Город успешно добавлен</p>
      </template>

      <template v-if="modalType === 'error'">
        <p>Ошибка</p>
        <div class="btns">
          <button @click="closeModal">Отмена</button>
          <button @click="addData">Повторить</button>
        </div>
      </template>
    </template>
  </BaseModal>
</template>

<script>
import BaseModal from "@/components/Modal/BaseModal";

export default {
  name: "CityModal",
  components: {
    BaseModal,
  },
  props: {
    modalType: {
      type: String,
      default: "load",
      validatitor: (value) => ["load", "success", "error", "default"].includes(value),
    },
  },

  data() {
    return {
      cityList: [],
      currentEl: "-1",
    };
  },

  mounted() {
    this.loadData();
  },

  methods: {
    closeModal() {
      this.$emit("closeModal");
    },

    addData() {
      this.$emit("addData", this.currentEl);
    },

    currentValue(value) {
      this.currentEl = this.cityList[value];
    },

    loadData() {
      setTimeout(() => {
        this.cityList = [
          {
            key: "99",
            name: "Белгород",
            region: "22",
            color: "890",
          },
          {
            key: "222",
            name: "Новгород",
            region: "71",
            color: "f80",
          },
          {
            key: "888",
            name: "Ростов",
            region: "51",
            color: "216",
          },
        ];
        this.$emit("changeType", "default");
      }, 1000);
    },
  },

  computed: {
    isDisabled() {
      if (this.currentEl === "-1") {
        return true;
      }

      return false;
    },
  },
};
</script>

<style></style>
