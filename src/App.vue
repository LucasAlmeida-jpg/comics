<template>
  <div>
    <Home />
  </div>
  <RouterView />

</template>

<script>
import Home from "@/components/Home.vue";

export default {
  components: {
    Home
  },
  data() {
    return {
      

    }
  },

};
</script>

<style scoped></style>