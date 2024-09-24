<template>
  <div class="app">
    <div class="header container h-100 p-5">
      <h1 class="mb-5">
        Weather-io
      </h1>
      <div class="d-flex justify-content-center h-100">
        <div class="searchbar w-50 mx-2">
          <input type="text" class="input form-control" v-model="city" placeholder="Enter your City">
        </div>
        <button class="btn-search btn btn-primary" @click="searchWeather">Search <i class="fas fa-search"></i>
        </button>
      </div>
    </div>
    <br>
    <WeatherComp :city="city" v-if="showWeather"></WeatherComp>
  </div>
</template>

<script>
import WeatherComp from './components/WeatherComp.vue';
export default {
  name: 'App',
  components: {
   WeatherComp
  },
  data( ){
    return {
      city: '',
      showWeather: false,
    }
  },
  methods:{
    async searchWeather () {
      this.showWeather = false;
      await this.$nextTick();
      this.showWeather = true;
    }
  }
}
</script>

<style>
body{
  background-color: rgb(24, 24, 24) !important;
}

.header{
  background-color: rgb(44, 47, 49);
  color: aliceblue;
  text-align: center;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  margin-top: 8rem;
}

.btn-search{
  background-image: linear-gradient(to right, rgb(83, 77, 77), rgb(43, 12, 12));
}

</style>
