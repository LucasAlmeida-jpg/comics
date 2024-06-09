<template>
  <div>
    <Modal />
  </div>
</template>

<script>
import Modal from "../components/Modal.vue";

export default {
  components: {
    Modal
  },
  data() {
    return {
      products: []
    };
  },

};
</script>

<style scoped>
</style>
