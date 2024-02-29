<template>
  <div :class="backgroundClass">
    <br>
    <br>
    <br>
    <br>
    <br>
    <div v-if="!weatherData">
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <p class="style4">Type in a city to see its current weather</p>
    <br>
    <input type="text" v-model="cityName" placeholder="Enter City Name">
    <button @click="fetchWeather">Get Weather</button>
    </div>
    <br>
    <br>
    <div v-if="error === 'City not found'">
      <img :src="notFoundIcon" alt="City not found" class="weather-icon" />
      <p class="style2">{{ error }}</p>
      <br>
    </div>
    <div v-else-if="weatherData">
      <h2 class="style1" >{{ weatherData.name }}</h2>
      <br>
      <img :src="weatherIcon" alt="Weather Icon" class="weather-icon" />
      <br>
      <div>
        <p class="style1">{{ weatherData.weather[0].main }}</p>
        <p class="style2">{{ weatherData.main.temp }}°C</p>
        <br>
        <p class="style2">Humidity: {{ weatherData.main.humidity }}%</p>
        <br>
        <p class="style2">Wind Speed: {{ weatherData.wind.speed }} km/h</p>
        <br>
        <p class="style2" >{{ weatherData.weather[0].description }}</p>
        <br>
        <br>
        <button v-if="weatherData" @click="resetSearch">Search for Another City</button>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
      </div>
    </div>
    <p v-if="error" class="style3">{{ error }}</p>
  </div>
</template>

<script>
import sunnyIcon from '@/assets/sunny-icon.png';  
import cloudyIcon from '@/assets/cloudy-icon.png';
import rainyIcon from '@/assets/rainy-icon.png';
import snowIcon from '@/assets/snow-icon.jpeg';
import mistIcon from '@/assets/mist-icon.png';

export default {
  data() {
    return {
      cityName: '', // Updated to cityName
      weatherData: null,
      error: null,
      notFoundIcon: require('@/assets/not-found-icon.png'), 
    };
  },
  computed: {
    backgroundClass() {
      if (!this.weatherData || !this.weatherData.weather) {
        return ''; // Default class if no weather data is available
      }
      const mainCondition = this.weatherData.weather[0].main.toLowerCase();
      if (mainCondition.includes('clear')) {
        return 'sunny';
      } else if (mainCondition.includes('cloud')) {
        return 'cloudy';
      } else if (mainCondition.includes('rain') || mainCondition.includes('thunderstorm')) {
        return 'rainy';
      } else if (mainCondition.includes('snow') || mainCondition.includes('thunderstorm')) {
        return 'snow';
      } else if (mainCondition.includes('mist') || mainCondition.includes('thunderstorm')) {
        return 'mist';
      } 
      return ''; // Default class for other conditions
    },

    weatherIcon() {
      if (!this.weatherData || !this.weatherData.weather) {
        return '';
      }
      const mainCondition = this.weatherData.weather[0].main.toLowerCase();
      switch (mainCondition) {
        case 'clear':
          return sunnyIcon;
        case 'clouds':
          return cloudyIcon;
        case 'rain':
          return rainyIcon;
        case 'thunderstorm':
          return rainyIcon;
        case 'snow':
          return snowIcon;
        case 'mist':
          return mistIcon;
        default:
          return '';
      }
    }
  },
  methods: {
    resetSearch() {
        this.weatherData = null;
        return;
    },

    fetchWeather() {
      if (!this.cityName) {
        this.error = 'Please enter a city name.';
        this.weatherData = null;
        return;
      }
      const apiKey = '101157b52aef4677753bbea680a0c65d'; // Ensure this is your actual API key
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.cityName}&units=metric&appid=${apiKey}`;

      fetch(url)
        .then(response => {
        if (!response.ok) {
            this.weatherData = null; // Reset weather data
            if (response.status === 404) {
            this.error = 'City not found'; // error message
            } else {
            throw new Error(`Failed to fetch: Server responded with status ${response.status}`);
            }
            throw new Error(this.error);
        }
        return response.json();
        })
        .then(data => {
          this.weatherData = data;
          this.error = null;
        })
        .catch(error => {
          console.error(error);
          this.weatherData = null;
        });
    },
  },
};
</script>

<style>
.sunny {
  background: url('~@/assets/sunny-img.jpeg') no-repeat center center fixed;
  background-size: cover; 
}

.cloudy {
  background: url('~@/assets/cloudy-img.webp') no-repeat center center fixed;
  background-size: cover; 
}

.rainy {
  background: url('~@/assets/rainy-img.webp') no-repeat center center fixed;
  background-size: cover; 
}

.snow {
  background: url('~@/assets/snow-img.avif') no-repeat center center fixed;
  background-size: cover;
}

.mist {
  background: url('~@/assets/mist-img.jpeg') no-repeat center center fixed;
  background-size: cover;
}

.weather-icon {
  display: block; 
  margin: 0 auto; 
  width: 100px; 
  height: auto; 
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body, html {
  font-family: 'Arial', sans-serif;
  text-align: center;
  padding: 20px;
}

.weather-icon {
  display: block;
  margin: 20px auto;
  width: 100px;
  height: auto;
}

input[type="text"] {
  padding: 10px;
  margin-top: 20px;
  border-radius: 5px;
  border: 1px solid #ccc;
  width: calc(100% - 22px);
  max-width: 300px; /* Adjust based on preference */
}

button {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-left: 10px;
}

button:hover {
  background-color: #0056b3;
}

/* Weather data styling */
.weather-data {
  margin-top: 20px;
  background-color: rgba(255, 255, 255, 0.8);
  padding: 20px;
  border-radius: 10px;
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
}

.weather-data h2 {
  margin-bottom: 15px;
}

.weather-data p {
  margin-bottom: 10px;
  font-size: 16px;
}

p[ v-if="error"] {
  color: #ff0000;
  margin-top: 20px;
}

.style1 {
  color: white;
  font-size: 30px; /* Adjust the size as needed */
  font-weight: bold;
  margin-bottom: 10px;
}

.style2 {
  color: white;
  font-size: 20px; /* Adjust the size as needed */
  margin-bottom: 10px;
}

.style4 {
  color: black;
  font-size: 20px; /* Adjust the size as needed */
  margin-bottom: 10px;
}

@media (max-width: 600px) {
  .weather-data, input[type="text"], button {
    width: 90%; /* Makes input and button take more width on smaller screens */
    margin-top: 10px;
  }
  
  .weather-icon {
    width: 80px; /* Adjusts icon size for smaller screens */
  }
  
  .style1, .style2 {
    font-size: 18px; /* Smaller font size for smaller screens */
  }
}
</style>
