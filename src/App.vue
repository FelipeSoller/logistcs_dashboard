<template>
  <div id="app">
    
  </div>
</template>

<script>

import axios from 'axios';

export default {
  name: 'App',
  components: {
    
  },
  async created() {
    const { data } = await axios.get('https://6050b01f5346090017670430.mockapi.io/api/routes');
    console.log(data);
  }
}
</script>

<style>

</style>
