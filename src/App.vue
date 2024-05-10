<template>
  <div class="weather" :class="weatherClass">
    <div class="container">
      <div class="card weather-form">
        <input class="weather-form__input" type="text" v-model="searchQuery" @keyup.enter="weatherSearch" placeholder="Enter city">
        <button class="weather-form__btn" @click="weatherSearch">Search</button>
      </div>

      <div class="card weather-load" v-if="loading">Loading...</div>

      <div class="weather-info" v-show="!error && location && temperature !== 0 && description">
        
        <div class="card" v-if="error">Error</div>
        
        <div class="weather-info__text">
          <div class="card">{{ location }}</div>
          <div class="card">{{ temperature }}*C</div>
          <div class="card">{{ description }}</div>
        </div>
      </div>
    </div>
    <div class="weather-bg">
      <div>
        <img class="weather-bg__img bg" src="./assets/main-bg.jpg" alt="main-bg">
        <img class="weather-bg__img overcast" src="./assets/cloudy-weather_bg.jpg" alt="main-bg">
        <img class="weather-bg__img partly-cloudy" src="./assets/neather-cloudy__bg.jpg" alt="main-bg">
        <img class="weather-bg__img sunny" src="./assets/weather_bg.jpg" alt="main-bg">
      </div>
    </div>
  </div>
</template>
<script>
// import { computed } from 'vue';
// Хуй знает сама по себе появилась

export default {
  data() {
    return {
      location: '',
      temperature: 0,
      description: '',
      loading: false,
      error: false,
      searchQuery: '',
    };
  },
  computed: {
    weatherClass() {
      if(this.description.includes('Sunny')) {
        return 'sunny';
      } else if(this.description.includes('Overcast')) {
        return 'overcast';
      } else if(this.description.includes('Partly cloudy')) {
        return 'partly-cloudy';
      } else {
        return '';
      }
    }
  },
  methods: {
    weatherSearch() {
      this.loading = true;
      this.error = false;
      fetch(`http://api.weatherapi.com/v1/current.json?key=2e11bc32dca643daab8190740240905&q=${this.searchQuery}`)
        .then(response => response.json())
        .then(data => {
          this.loading = false;
          this.location = data.location.name;
          this.temperature = data.current.temp_c;
          this.description = data.current.condition.text;
          this.resetSearchQuery();
        })
        .catch(error => {
          this.loading = false;
          this.error = true;
        })
    },
    resetSearchQuery() {
      this.searchQuery = '';
    }
  }
}
</script>