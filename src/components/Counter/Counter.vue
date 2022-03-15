<template>
  <div class="wrap">
    <div class="btns">
      <app-button @click="increaseCounter">+</app-button>
      <input
        :class="{
          error: error,
        }"
        type="text"
        placeholder="Введите данные"
        v-model="counter"
        maxlength="2"
        max="10"
        @input="getInputValue"
      />
      <app-button @click="decreaseCounter">-</app-button>
    </div>
    <span v-if="error">{{ error }}</span>
  </div>
</template>

<script>
import AppButton from "@/components/base/SmallButton.vue";

export default {
  name: "Counter",

  components: {
    AppButton,
  },

  data() {
    return {
      counter: "",
      error: "",
    };
  },

  methods: {
    increaseCounter() {
      if (this.counter < 10) {
        this.counter++;
        this.$emit("counterValue", this.counter);
      }
    },
    decreaseCounter() {
      if (this.counter > 1) {
        this.counter--;
        this.$emit("counterValue", this.counter);
      }
    },
  },

  computed: {},

  methods: {
    getInputValue(event) {
      let value = event.target.value;
      this.$emit("counterValue", value);
    },
  },
};
</script>

<style>
.btns {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
  justify-content: space-between;
}

span {
  display: block;
  text-align: center;
  color: #f00;
}

.wrap {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  max-width: 300px;
}

input {
  outline: transparent;
}

.error {
  border: 1px solid #f00;
}
</style>
