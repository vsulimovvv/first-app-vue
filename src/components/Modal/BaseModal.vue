<template>
  <div class="popup">
    <div class="popup__container">
      <button class="popup__close" type="button" aria-label="Закрыть" @click="closeModal">X</button>

      <div class="popup__wrapper">
        <h3 class="popup__title">
          <slot name="title"></slot>
        </h3>

        <div class="popup__content">
          <slot name="content"></slot>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "BaseModal",
  props: {
    modalType: {
      type: String,
      default: "load",
      validatitor: (value) => ["load", "success", "error", "default"].includes(value),
    },
  },
  emits: ["closeModal"],

  data() {
    return {};
  },

  methods: {
    closeModal() {
      this.$emit("closeModal");
    },
  },

  computed: {},
};
</script>

<style lang="scss">
.popup {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: rgba(0, 0, 0, 0.8);
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
  transition: 350ms ease;

  &.active {
    animation: fadeIn 500ms ease forwards;
    display: flex;
  }

  &__container {
    width: calc(100% - 40px);
    max-width: 380px;
    transition: 250ms ease-in;
    position: relative;
    padding: 20px;
    border-radius: 10px;

    background-color: #f8f8f8;

    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
  }

  &__wrapper {
    width: 100%;
  }

  &__title {
    font-weight: 700;
    margin: 0 auto;
    line-height: 1.3;
    text-align: center;
    font-weight: 300;
    max-width: 380px;
    font-size: 30px;
    margin-bottom: 30px;
  }

  &__select {
    margin-bottom: 30px;
    padding: 10px;
  }

  &__btns {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-around;
  }

  &__btn {
    padding: 10px 20px;
    border-radius: 5px;
    background-color: #2abb4c;
    color: #fff;

    &:disabled {
      cursor: not-allowed;
      background-color: darken(#2abb4c, 15%);
      &:hover {
        background-color: darken(#2abb4c, 15%);
      }
    }

    &:hover {
      background-color: darken(#2abb4c, 5%);
    }

    &--cancel {
      background-color: #1f1f1f;

      &:hover {
        background-color: lighten(#1f1f1f, 5%);
      }
    }

    &:active {
      transform: scale(0.98);
    }
  }

  &__close {
    position: absolute;
    right: 10px;
    top: 10px;
    fill: #838383;
    transition: 150ms ease;
  }

  @keyframes fadeIn {
    0% {
      opacity: 0;
      visibility: hidden;
    }

    100% {
      opacity: 1;
      visibility: visible;
    }
  }
}
</style>
