<template>
  <div class="container p-0">
    <div class="d-flex">
      <div class="card main-div w-100">
        <div class="p-3">
          <h2 class="mb-1 day">
            Today
          </h2>
          <p class="text-light date mb-0">
            {{date}}
          </p>
          <small>{{time}}</small>
          <h2 class="place"><i class="fa fa-location">{{ name }} <small>{{country}}</small></i></h2>
          <div class="temp">
            <h1 class="weather-temp">{{ tempreture }}&deg;</h1>
            <h2 class="text-light">{{ description}}<img :src="iconUrl"></h2>
          </div>
        </div>
      </div>
      <div class="card card-2 w-100">
    <table class="mb-4">
      <tbody>
        <tr>
          <th>sealvl</th>
          <td>100</td>
        </tr> 
        
        <tr>
          <th>sealvl</th>
          <td>100</td>
        </tr>
        
        <tr>
          <th>sealvl</th>
          <td>100</td>
        </tr>
        
      </tbody>
    </table>

    <DaysWeather></DaysWeather>

    <div id="div_Form" class="d-flex m-3 justify-content-center">
      <form action=""></form>
      <input type="button" value="Change Location" class="btn change-btn btn-primary">
    </div>
    </div>

    </div>
  </div>
</template>

<script>
import axios from 'axios';
import DaysWeather from './DaysWeather.vue';

export default {
  name: 'WeatherComp',
  components: {
    DaysWeather,
  },
  props: {
    city: String,
  },
  data() {
    return {
      tempreture: null,
      description: null,
      iconUrl: null,
      date: null,
      time: null,
      name: null,
      monthNames: ["January", "Febuary", "March", "April", "May",  "June",  "July", "August", "September",  "October", "November", "December"],
    };
  },
  async created() {
    try {
      const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=1be0726ff1c0f86a8189955995d6e65a`);
      const weatherData = response.data;
      console.log(weatherData);
      this.tempreture = Math.round(weatherData.main.temp);
      this.description = weatherData.weather[0].description;
      this.iconUrl = `https://openweathermap.org/img/wn/${weatherData.weather[0].icon}.png`;
      const d = new Date();
      this.date = d.getDate() + '-' + this.monthNames[d.getMonth()] + '-' + d.getFullYear();
      this.time =d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds();
    } catch (error) {
      console.error('Error fetching weather data:', error);
    }
  }
}

</script>

<style>
body{
  background-color: grey;
}


.weather-temp{
  margin: 0;
  font-weight: 700;
  font-size: 4em;
}

h2.mb-1.day{
  font-size: 3rem;
}

.main-div{
  border-radius: 30px;
  color: #fff;
  background-image: url("https://zubayrlatief.github.io/Images-/capetownimage.jpg");
  background-size: cover;
  background-position: center;
  background-color: black;
  background-repeat: no-repeat;
}

.temp {
  position: absolute;
  bottom: 0;
}
.main-div:hover{
  transform: scale(1.1);
  transition: transfrom 0.5s ease;
  z-index: 1;
}
.card.card-2 {
  background:rgb(44, 47, 49);
  border-radius: 20px;
}
.card-details{
  margin-left: 18px;
}

.h1_left{
  position: absolute;
  bottom: 25px;
  left: 16px;
  font-size: 3vw;
  line-height: 1.2;
}
h3_left{
  position: absolute;
  left: 16px;
  font-size: 2vw;
  line-height: 0.5;

}
h3_left small{
  font-size: 1rem;
}

table {
  position: relative;
  left: 15px;
  border-collapse: separate;
  border-spacing: 15px;
  width: 85%;
  text-align: left;
  max-width: 600px;
  margin: 0 auto;

}

th, td {
  font-size: 18px;
  color: white;
   }

   td{
    text-align: right;
   }

  table, tr:hover {
    color: red;
  }
  .change_btn{
    background-image: linear-gradient(to right,cyan, magenta);
  }
  .change_btn:hover{
    transform: scale(0.9);
    transition: transform 0.1s ease;
  }
</style>
