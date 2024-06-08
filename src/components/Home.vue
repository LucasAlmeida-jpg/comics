<template>
  <div>
    <Modal :allProducts="products" />
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

  mounted() {
        axios.get('http://localhost:3000/comics')
            .then(response => {
                if (response.data && Array.isArray(response.data)) {
                    this.products = response.data;
                    console.log(this.products, 'aqyu');
                }
            })
            .catch(error => {
                console.error('Erro ao carregar dados do produto:', error);
            });
    },
};
</script>

<style scoped>
</style>
