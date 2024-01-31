<template>
  <div class="fixed inset-0 flex items-center justify-center">
    <div class="absolute inset-0 bg-black opacity-50"></div>
    <div class="relative bg-white p-4 rounded-md w-72">
      <button @click="closePopup" class="absolute top-0 right-0 m-2 text-gray-600 hover:text-gray-800">
        <svg fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
          viewBox="0 0 24 24" class="w-6 h-6">
          <path d="M6 18L18 6M6 6l12 12"></path>
        </svg>
      </button>
      <form @submit.prevent="submitForm">
        <label for="name" class="mb-2 block">Имя:</label>
        <input type="text" v-model="formData.name" required class="w-full p-2 mb-4 border border-gray-300 rounded-md">

        <label for="phone" class="mb-2 block">Телефон:</label>
        <InputMask
          v-model="formData.phone"
          placeholder="+7 (999) 999-9999"
          mask="+7 (999) 999-9999"
          required
          class="w-full p-2 mb-4 border border-gray-300 rounded-md"
        />
  
        <label for="email" class="mb-2 block">Email:</label>
        <input type="email" v-model="formData.email" required class="w-full p-2 mb-4 border border-gray-300 rounded-md">
  
        <label for="city" class="mb-2 block">Город:</label>
        <select v-model="formData.city" required class="w-full p-2 mb-8 border border-gray-300 rounded-md">
          <option v-for="city in cities" :value="city.name" :key="city.id">{{ city.name }}</option>
        </select>
  
        <button type="submit" class="bg-blue-500 text-white font-bold py-2 px-4 rounded-full">
					Отправить
				</button>
      </form>
      <div v-if="responsePopup" class="fixed inset-0 flex items-center justify-center">
        <div class="absolute inset-0 bg-black opacity-50"></div>
        <div class="relative bg-white p-4 rounded-md">
          <p>{{ responseMessage }}</p>
			    <button 
						@click="closeResponsePopup" 
						class="bg-blue-500 text-white font-bold py-2 px-4 rounded-full mt-4"
						>Закрыть
					</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import  InputMask  from 'primevue/inputmask';
export default {
  components: {
    InputMask,
  },
  data() {
    return {
      formData: {
        name: "",
        phone: "",
        email: "",
        city: ""
      },
      cities: [
        { id: 1, name: "Москва" },
        { id: 2, name: "Санкт-Петербург" },
        { id: 3, name: "Казань" }
      ],
      responsePopup: false,
      responseMessage: ''
    };
  },
  methods: {
    closePopup() {
      this.formData = {
        name: "",
        phone: "",
        email: "",
        city: ""
      };
      this.$emit("close");
    },
    getCitiesIdByName(cityName) {
      const city = this.cities.find(city => city.name === cityName);
      return city ? city.id : null;
    },
    submitForm() {
      axios.post('http://hh.autodrive-agency.ru/testtasks/front/task-7/', {
        name: this.formData.name,
        phone: this.formData.phone,
        email: this.formData.email,
        city_id: this.getCitiesIdByName(this.formData.city)
      })
      .then(response => {
        this.responseMessage = 'Данные успешно отправлены на сервер!';
        this.responsePopup = true;
        this.closePopup();
      })
      .catch(error => {
        this.responseMessage = 'Ошибка отправки данных на сервер.';
        this.responsePopup = true;
      });
    },

    closeResponsePopup() {
      this.responsePopup = false;
      this.responseMessage = '';
    },

  },
  mounted() {
    if (this.$props.city) {
      this.formData.city = this.$props.city;
    }
  },
  watch: {
    city(newCity) {
      this.formData.city = newCity;
    }
  },
  props: {
    city: String
  }
};
</script>
  
<style scoped>

</style>
  