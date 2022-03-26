<template>
  <CityList :cityList="dataCities" />
  <CityModal
    @closeModal="closeModal"
    v-if="showModalCity"
    @addData="addDataCity"
    :modalType="modalType"
    @changeType="changeType"
  />
  <ColorModal
    @closeModal="closeModal"
    v-if="showModalColor"
    @addData="addDataColor"
    :modalType="modalType"
  />
</template>

<script>
import CityModal from "@/components/Modal/CityModal";
import ColorModal from "@/components/Modal/ColorModal";
import CityList from "@/components/CityList/CityList";

export default {
  name: "ModalPage",

  components: {
    CityModal,
    ColorModal,
    CityList,
  },

  data() {
    return {
      showModalCity: true,
      showModalColor: false,
      modalType: "load",

      dataCities: [],
    };
  },

  mounted() {
    this.fetchData();
  },

  methods: {
    async fetchData() {
      setTimeout(() => {
        this.dataCities = [
          {
            key: "9",
            name: "Москва",
            region: "199",
            color: "808080",
          },
          {
            key: "2",
            name: "Анапа",
            region: "78",
            color: "f00",
          },
          {
            key: "333",
            name: "Киров",
            region: "1",
            color: "333333",
          },
          {
            key: "222",
            name: "Калуга",
            region: "33",
            color: "cc0eee",
          },
        ];
      }, 2000);
    },

    closeModal() {
      this.showModalCity = false;
      this.showModalColor = false;
    },

    addDataCity(city) {
      this.dataCities.push(city);
      const isSuccess = Math.random() > 0.5;

      if (isSuccess) {
        this.modalType = "success";
      } else {
        this.modalType = "error";
      }
    },

    addDataColor(colorValue) {
      this.closeModal();
      console.log(colorValue);
    },
    changeType(type) {
      this.modalType = type;
    },
  },

  computed: {},
};
</script>

<style lang="scss">
// ! Сделать стили для слота
</style>
