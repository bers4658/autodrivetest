<template>
  <button @click="handleClick" class="btn bg-blue-500 hover:bg-blue-700 text-white font-bold py-5 px-4 rounded">
    Заказать в город {{ city }}
  </button>
</template>

<script>
export default {
  props: {
    city: String
  },
  methods: {
    handleClick() {
      this.$emit('open-popup', this.city);
    }
  }
};
</script>