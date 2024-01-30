<template>
  <div id="app" class="flex items-center justify-center h-screen">
    <CityButton city="Москва" @open-popup="openPopup" />
    <div class="mx-4"></div>
    <CityButton city="Санкт-Петербург" @open-popup="openPopup" />

    <PopupForm v-if="showPopup" :city="selectedCity" @close="closePopup" />
  </div>
</template>

<script>
import PopupForm from "./components/PopupForm.vue";
import CityButton from "./components/CityButton.vue";

export default {
  data() {
    return {
      showPopup: false,
      selectedCity: ""
    };
  },
  methods: {
    openPopup(city) {
      this.selectedCity = city;
      this.showPopup = true;
    },
    closePopup() {
      this.showPopup = false;
      this.selectedCity = "";
    }
  },
  components: {
    PopupForm,
    CityButton
  }
};
</script>

<style lang="postcss">
@import './styles/tailwind.css';
</style>
