<template>
    <div class="days-tab text-center">
      <div v-if="loading" class="loading">Loading..</div>
      <ul v-if="!loading" class="p-0">
        <li v-for="(day, index) in weatherData" :key="index" class="li_active">
          <div class="py-3">
            <img :src="day.iconUrl" alt="weather icon">
          </div>
          <div class="py-3">{{ getDayName(day.date) }}</div> <!-- Pass day.date here -->
          <div class="py-3">{{ day.temperature }}°C</div>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import moment from 'moment';
  
  export default {
    props: {
      cityname: {
        type: String,
        default: 'New York' // Default city if cityname prop is not provided
      }
    },
    data() {
      return {
        weatherData: [],
        loading: true
      };
    },
    mounted() {
      this.fetchWeatherData();
    },
    methods: {
      async fetchWeatherData() {
        const apiKey = '1be0726ff1c0f86a8189955995d6e65a';
        const city = this.cityname;
        const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}`;
  
        try {
          const response = await axios.get(apiUrl);
          const forecastData = response.data.list;
  
          const filteredData = forecastData
            .map(item => {
              return {
                date: moment(item.dt_txt.split(' ')[0]).format('YYYY-MM-DD'), // Keep date in a standard format for logic use
                temperature: Math.round(item.main.temp),
                iconUrl: `https://openweathermap.org/img/wn/${item.weather[0].icon}.png`
              };
            })
            .reduce((acc, item) => {
              if (!acc.some(day => moment(day.date).isSame(item.date, 'day'))) {
                acc.push(item);
              }
              return acc;
            }, [])
            .slice(0, 4); // Get the next 4 days
  
          this.weatherData = filteredData;
          this.loading = false;
        } catch (error) {
          console.error('Error fetching weather data', error);
          this.loading = false;
        }
      },
      getDayName(date) { // Accept date as a parameter
        return moment(date).format('ddd'); // Format date to show the day name
      }
    }
  };
  </script>
  
<style>
.days-tab{
    width: 90%;
    box-shadow: 0 4px 8px black, 0 6px 20px black;
    border-radius: 20px;
    width: 90%;
    margin: auto;
}

.loading{
    color: white;
}

ul{
    margin: 0;
}

li{
    display: inline-block;
    list-style: none;
    height: 100%;
    width: 21%;
    max-width: 21%;
    font-size: 1vw;
    line-height: 1.2;
}
span{
    display: block;
    margin-bottom: 5px;
    font: 100% sans-serif;
    height: 35px;
}
.li_active{
    color: rgb(81, 77, 77);
    background: grey;
    border-radius: 10px;
    margin: 0.5rem;
    color: white;
    font-weight: 600;

}
.li_active:hover{
    transform: scale(1,2);
    transition: transform 0.1s ease;
}
.li_active_temp{
    display: inline-block;
    background-color: grey;
    color: white;
    transition: background-color 0.5s;
    border-radius: 10px;
}

.li_active_temp:hover{
    transform: scale(1.2);
    transition: transform 0.1s ease;
    background: fff;
    border-radius: 10px;
    color: black;
}
</style>