<template>
  <div class="calc">
    <h2 class="calc__title">Калькулятор</h2>
    <div class="calc__wrapper" v-if="!error && !globalError">
      <div>
        Введите 1 число
        <Counter @counterValue="getNumberOne" />
      </div>
      <div class="calc__operand">
        Выберите операнд
        <MySelect :options-list="myOptions" @onChange="getOperand" />
      </div>
      <div>
        Введите 2 число
        <Counter @counterValue="getNumberTwo" />
      </div>

      <AppButton button-name="Calculate" @onButtonClick="calculate" />
      <p class="calc__result" v-if="result">Результат: {{ result }}</p>
    </div>

    <div v-if="error">
      <p class="calc__error">Ошибка: {{ error }}</p>
      <p class="calc__error">Ошибка: {{ globalError }}</p>
    </div>
  </div>
</template>

<script>
import Counter from "@/components/Counter/Counter";
import MySelect from "@/components/base/MySelect";
import AppButton from "@/components/base/AppButton";

export default {
  name: "Calculator",

  components: {
    Counter,
    MySelect,
    AppButton,
  },

  props: {},

  data() {
    return {
      myOptions: [
        {
          name: "Умножение",
          value: "multiple",
        },
        {
          name: "Деление",
          value: "division",
        },
        {
          name: "Вычитание",
          value: "subtraction",
        },
        {
          name: "Сложение",
          value: "addition",
        },
      ],

      numberOne: "",
      numberTwo: "",
      operandName: "",
      typeCalc: "",
      error: "",
      globalError: "",
      result: "",
    };
  },

  methods: {
    getNumberOne(value) {
      this.numberOne = value;
      this.getError(value);
    },
    getNumberTwo(value) {
      this.numberTwo = value;
    },
    getOperand(value) {
      this.operandName = value;
    },
    getError(number) {
      if (number > 1 && number <= 10) {
        this.error = "Неверный диапозон";
      }
      // ! Проверить на тип number
      if (Number(number) === typeof "number") {
        this.error = "Неверный значение";
      }
    },

    calculate() {
      console.log(this.numberOne, this.numberTwo, this.operandName);
      if (!this.error && this.numberOne && this.numberTwo && this.operandName) {
        if (this.operandName === "multiple") {
          this.result = this.numberOne * this.numbertwo;
        }
        if (this.operandName === "division") {
          this.result = this.numberOne / this.numbertwo;
        }
        if (this.operandName === "subtraction") {
          this.result = this.numberOne - this.numbertwo;
        }
        if (this.operandName === "addition") {
          this.result = this.numberOne + this.numbertwo;
        }
      } else {
        this.globalError = "Все сломано";
      }
    },
  },

  computed: {},
};
</script>

<style>
.calc__wrapper {
  display: flex;
  align-content: center;
  gap: 20px;
}

.calc__operand {
  display: flex;
  flex-direction: column;
  max-width: 200px;
}

@media screen and (max-width: 767px) {
  .calc__wrapper {
    flex-direction: column;
  }
}
</style>
