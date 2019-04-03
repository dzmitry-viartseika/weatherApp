<template>
  <v-app>
    <v-toolbar app color="rgba(255,255,255,0.3)">
      <v-toolbar-title class="headline text-uppercase">
        <span class="title font-weight-bold">WEATHER APP</span>
      </v-toolbar-title>
    </v-toolbar>

    <v-content>
      <v-form @submit.prevent="getData">
        <v-container>
            <v-text-field
              label="Enter your city"
              prepend-inner-icon="place"
              v-model.trim="city"
              color="white"
              counter="20"
              style="font-size: 20px;color: white"
            ></v-text-field>

          <v-btn style="margin: 10px auto;display: block;" color="error" fab large dark
          @click.prevent="getData"
          >
            <v-icon>search</v-icon>
          </v-btn>
        </v-container>
      </v-form>
      <div class="information">
        <h2>{{city}}</h2>
        <img src="@/assets/thermometer.svg">
        <p>{{currentTemp}}°</p>
        <span id="temp-values">Min {{minTemp}}°  Max {{maxTemp}}°</span>
      </div>
      <div class="information flex">

        <div class="flex-block">
          <img class="icon" src="@/assets/sunrise.svg">
          <h6>sunrise</h6>
          <p>{{sunrise}}</p>
        </div>
        <div class="flex-block">
          <img class="icon" src="@/assets/sunset.svg">
          <h6>sunset</h6>
          <p>{{sunset}}</p>
        </div>
        <div class="flex-block">
          <img class="icon" src="@/assets/humidity.svg">
          <h6>humidity</h6>
          <p>{{humidity}}</p>
        </div>
        <div class="flex-block">
          <img class="icon" src="@/assets/pressure.svg">
          <h6>pressure</h6>
          <p>{{pressure}}</p>
        </div>
        <div class="flex-block">
          <img class="icon" src="@/assets/windmill.svg">
          <h6>wind</h6>
          <p>{{wind}}</p>
        </div>








      </div>
    </v-content>

  </v-app>
</template>

<script>

  import axios from 'axios'

export default {
  name: 'App',
  components: {

  },
  data () {
    return {
      city: '',
      currentTemp: '',
      minTemp: '',
      maxTemp:'',
      sunrise: '',
      sunset: '',
      pressure: '',
      humidity: '',
      wind: '',
      overcast: '',
    }
  },
  methods: {
    getData() {
      let city = this.city
      let url = `http://api.openweathermap.org/data/2.5/weather?q=${this.city}&?units=metric&APPID=8077fb3361194aa15524ba0dfa759cb6`;
      axios.get(url)
        .then(response => {
          this.currentTemp = Math.floor(response.data.main.temp) - 273;
          this.minTemp = Math.floor(response.data.main.temp_min) - 273;
          this.maxTemp =  Math.floor(response.data.main.temp_max) - 273;
          this.pressure = response.data.main.pressure;
          this.humidity = response.data.main.humidity + '%';
          this.wind = response.data.wind.speed + 'm/s';
          this.overcast = response.data.weather[0].description;
          this.sunrise = new Date(response.data.sys.sunrise*1000).toLocaleTimeString("ru-RU").slice(0,4);
          this.sunset = new Date(response.data.sys.sunset*1000).toLocaleTimeString("ru-RU").slice(0,4);
          console.log(response.data);
        })
        .catch(error => {
          console.log(error)
        })
    }
  }
}
</script>


<style>
  .button {
    display: block;
    margin: 0 auto;
  }

  img {
    width: 50px;
    height: 50px;
    margin: 15px;
  }

  ul {
    list-style-type: none;
  }

  .information {
    margin: 10px auto;
    text-align: center;
  }

  li {
    padding: 5px;
  }

  .flex {
    display: flex;
    justify-content: center;
    flex-direction: row;
  }

  .flex-block {
    margin: 25px;
  }

  #app {
    background-image: linear-gradient(to left top, #7f9dca, #9788d0, #c467ba, #e73883, #eb1236);
    color: white;
  }

  .toolbar {
    background-color: none transparent;
  }

  .title {
    color: white;
  }

  .input {
    color: white;
    border: 1px solid white;
  }

</style>
