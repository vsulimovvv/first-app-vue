<template>
  <div class="container">
    <h1>Криптовалюта</h1>

    <div class="head">
      <label class="head__label" for="">Тикер</label>
      <input
        class="head__input"
        type="text"
        placeholder="Введите название валюты"
        v-model="searchCurrency"
        @input="showHints"
      />

      <ul class="head__list" v-if="hint">
        <li class="head__item">
          <span class="head__link" @click="addNewCoinCard">{{ hint }}</span>
        </li>
      </ul>

      <p>
        Общая стоимость: <strong>{{ amount }} $</strong>
      </p>

      <!-- <button class="head__btn btn" type="button" @click="addNewCoinCard">Добавить</button> -->
    </div>
    <hr />

    <div class="sort">
      <div class="sort__btns">
        <button
          class="sort__btn btn"
          type="button"
          @click="sortCardsByName('increase')"
          :class="'sort__btn--active'"
        >
          По имени A-Z
        </button>
        <button class="sort__btn btn" type="button" @click="sortCardsByName('decrease')">
          По имени Z-A
        </button>
        <button class="sort__btn btn" type="button" @click="sortCardsByPrice('increase')">
          По возрастанию
        </button>
        <button class="sort__btn btn" type="button" @click="sortCardsByPrice('decrease')">
          По убыванию
        </button>
      </div>

      <div class="sort__filter">
        <input
          type="text"
          placeholder="Фильтр по значению"
          @input="filterCoinByName"
          v-model="filterCurrency"
        />
      </div>
    </div>

    <hr />

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

    <hr />

    <div class="body">
      <ul class="body__list">
        <li class="body__item" v-for="(item, index) in pagination" :key="index">
          <h3 class="body__heading">{{ item[0] }}</h3>
          <p class="body__price">{{ item[1].value }} $</p>

          <button class="body__btn btn" @click="deleteCard(index)">Удалить</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "CryptocurrencyPage",

  data() {
    return {
      searchCurrency: "",
      filterCurrency: "",
      currencyList: [],
      hint: "",
      addedList: [],
      currentPage: 1,
      amountCards: 9,
    };
  },

  mounted() {
    this.fetchData();
  },

  methods: {
    fetchData() {
      fetch("https://api.coingecko.com/api/v3/exchange_rates")
        .then((response) => response.json())
        .then((data) => {
          let dataArr = Object.entries(data.rates).map((el, index) => {
            if (typeof String(el[0]) !== "string") {
              el[0] = "";
            }

            let { name, unit, value, type } = el[1];

            el[1].name = String(name) ?? "";
            el[1].unit = String(unit) ?? "";
            el[1].value = Number(value) ?? 0;
            el[1].type = String(type) ?? "";

            return el;
          });

          return dataArr;
        })
        .then((validData) => (this.currencyList = validData));
    },

    deleteCard(index) {
      this.currencyList.splice(index, 1);
    },

    sortCardsByName(sortType) {
      this.currencyList.sort((a, b) => {
        if (sortType === "increase") {
          if (a[1].name > b[1].name) {
            return 1;
          }
          if (a[1].name < b[1].name) {
            return -1;
          }
        }
        if (sortType === "decrease") {
          if (a[1].name > b[1].name) {
            return -1;
          }
          if (a[1].name < b[1].name) {
            return 1;
          }
        }
        return 0;
      });
    },

    sortCardsByPrice(sortType) {
      this.currencyList.sort((a, b) => {
        if (sortType === "increase") {
          if (a[1].value > b[1].value) {
            return 1;
          }
          if (a[1].value < b[1].value) {
            return -1;
          }
        }
        if (sortType === "decrease") {
          if (a[1].value > b[1].value) {
            return -1;
          }
          if (a[1].value < b[1].value) {
            return 1;
          }
        }
        return 0;
      });
    },

    showHints() {
      if (this.searchCurrency) {
        let filtered = this.currencyList
          .filter((el) => String(this.searchCurrency.toLowerCase()) === String(el[0].toLowerCase()))
          .map((el) => el[0]);

        if (filtered.length) {
          // !
          this.hint = filtered[0];
        } else {
          this.hint = "";
        }
      }
    },

    addNewCoinCard() {
      let name = this.currencyList.find(
        (el) => String(el[0]).toLowerCase() === String(this.hint).toLowerCase()
      );

      if (name !== undefined) {
        this.addedList.push(name);
      }

      this.hint = "";
      this.searchCurrency = "";
    },

    filterCoinByName() {
      if (this.filterCurrency) {
        let newArr = this.addedList.filter((el) =>
          String(el[1].name.toLowerCase()).includes(this.filterCurrency)
        );

        this.addedList = newArr;
      }
    },

    changePage(cardId) {
      this.currentPage = cardId;
    },

    // changeCardsOnPage() {},
  },

  computed: {
    amount() {
      let amount = this.currencyList.reduce((acc, current) => {
        return (acc += current[1].value);
      }, 0);

      return Math.round(amount);
    },

    pagination() {
      return this.currencyList.slice(this.startIndex, this.endIndex);
    },

    paginationCalc() {
      return Math.round(this.currencyList.length / this.amountCards);
    },

    startIndex() {
      return (this.currentPage - 1) * this.amountCards;
    },

    endIndex() {
      return this.currentPage * this.amountCards;
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  max-width: 1000px;
  margin: 0 auto;
}

h1 {
  text-align: center;
  margin-bottom: 10px;
  font-size: 30px;
}

a {
  text-decoration: none;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  padding: 0;
  margin: 0;
}

input {
  outline: transparent;
  border: none;
  border: 1px solid #999;
  padding: 10px;
  border-radius: 10px;
  color: #000;

  &::placeholder {
    color: #888;
  }

  &:focus {
    border-color: #000;
  }
}

.btn {
  font-family: Arial, Helvetica, sans-serif;
  color: #fff;
  font-size: 16px;
  padding: 10px 20px;
  cursor: pointer;
  border: none;
  outline: none;
  transition: 150ms ease, transform 0s;

  &:active {
    transform: scale(0.98);
  }
}

.head {
  padding: 15px 0;

  @media screen and (max-width: 769px) {
    padding: 10px 0;
  }

  &__label {
    display: block;
    font-size: 14px;
    margin-bottom: 10px;
  }

  &__btn {
    background-color: rgb(29, 141, 29);
    border-radius: 20px;
    margin-top: 20px;

    &:hover {
      background-color: lighten(rgb(29, 141, 29), 5%);
    }
  }

  &__list {
    display: flex;
    align-items: center;
    gap: 7px;
    padding: 10px 0;
  }

  &__link {
    display: inline-block;
    background-color: bisque;
    padding: 8px 15px;
    border-radius: 10px;
    color: #000;
    transition: 150ms ease, transform 0s;
    cursor: pointer;

    &:hover {
      background-color: darken(bisque, 20%);
      color: lighten(bisque, 10%);
    }

    &:active {
      transform: scale(0.98);
    }
  }
}

.body {
  width: 100%;
  padding: 15px 0;

  @media screen and (max-width: 769px) {
    padding: 10px 0;
  }

  &__list {
    display: grid;
    grid-template-columns: repeat(3, minmax(auto, 1fr));
    gap: 30px;

    @media screen and (max-width: 769px) {
      grid-template-columns: repeat(2, minmax(auto, 1fr));
      gap: 20px;
    }
  }

  &__item {
    border: 3px solid #999;
    display: flex;
    flex-direction: column;
    align-items: center;
    box-sizing: border-box;
    background-color: #fff;
    transition: 250ms ease;
    min-height: 150px;
    justify-content: space-around;
    text-align: center;
    box-sizing: border-box;

    * {
      box-sizing: inherit;
    }

    &:hover {
      border-color: #563d7c;
      transform: scale(1.02) translateY(-2px);
    }
  }

  &__heading,
  &__price {
    width: 100%;
    padding: 20px 10px;
    font-weight: 700;
    font-size: 20px;
    margin: 0;

    @media screen and (max-width: 769px) {
      font-size: 16px;
    }
  }

  &__heading {
    text-transform: uppercase;
    font-size: 24px;

    @media screen and (max-width: 769px) {
      font-size: 20px;
    }
  }

  &__price {
    font-weight: 600;
  }

  &__btn {
    margin-top: auto;
    width: 100%;
    background-color: #f1f1f1;
    border-top: 1px solid #222;
    color: rgb(163, 163, 163);
    font-size: 13px;
    text-transform: uppercase;
    letter-spacing: 0.2em;
    font-weight: 700;
    padding: 10px;

    &:hover {
      background-color: #563d7c;
      color: #fff;
      border-color: #563d7c;
    }
  }
}

.sort {
  padding: 15px 0;

  @media screen and (max-width: 769px) {
    padding: 10px 0;
  }

  &__btns {
    display: grid;
    grid-template-columns: repeat(4, auto);
    gap: 20px;
    margin-bottom: 20px;

    @media screen and (max-width: 769px) {
      grid-template-columns: repeat(2, auto);
      gap: 10px;
    }
  }

  &__btn {
    border-radius: 10px;
    background-color: #a3b2fa;
    color: #000;

    @media screen and (max-width: 576px) {
      font-size: 14px;
    }

    &--active,
    &:hover {
      background-color: #563d7c;
      color: #fff;
    }
  }

  &__filter {
    max-width: 300px;
    width: 100%;

    @media screen and (max-width: 769px) {
      max-width: 95%;
    }

    input {
      width: 100%;
    }
  }
}

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
