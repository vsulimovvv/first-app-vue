<template>
  <BaseModal @closeModal="closeModal">
    <template v-slot:title>Выберите цвет</template>

    <template v-slot:content>
      <template v-if="modalType === 'default'">
        <div class="popup__colors">
          <input class="popup__color" @change="currentColor = $event.target.value" type="color" />

          <div class="popup__btns">
            <button class="popup__btn popup__btn--cancel" @click="closeModal" type="button">
              Отмена
            </button>
            <button class="popup__btn" @click="addData" type="button" :disabled="isDisabled">
              Добавить
            </button>
          </div>
        </div>
      </template>

      <template v-if="modalType === 'load'">
        <p>Загрузка</p>
      </template>
      <template v-if="modalType === 'error'">
        <p>Ошибка</p>
        <div class="btns">
          <button>Отмена</button>
          <button>Повторить</button>
        </div>
      </template>
      <template v-if="modalType === 'success'">
        <p>Цвет успешно добавлен</p>
      </template>
    </template>
  </BaseModal>
</template>

<script>
import BaseModal from "@/components/Modal/BaseModal";

export default {
  name: "ColorModal",
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
      currentColor: "",
    };
  },

  methods: {
    closeModal() {
      this.$emit("closeModal");
    },

    addData() {
      this.$emit("addData", this.currentColor);
    },
  },

  computed: {
    isDisabled() {
      if (this.currentColor === "") {
        return true;
      }

      return false;
    },
  },
};
</script>

<style lang="scss" scoped>
.popup {
  &__color {
    margin-bottom: 30px;
  }
}
</style>
