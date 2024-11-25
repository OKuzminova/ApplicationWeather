<template>
  <div className="weather">
    <h2> Weather in {{ name == "" ? "your city" : name }} </h2>
    <div className="weather-search">
      <input type="text" v-model="name" placeholder="City">
      <button @click="getWearher()">Search</button>
    </div>
    <p> {{ error }}</p>
    <div className="weather-info" v-if="info != null">
      <div className="weather-info__item">
        <p>Temperature:<br>
          <strong>{{ showTemp }}</strong>
        </p>
      </div>
      <div className="weather-info__item">
        <p>Feels like:<br>
          <strong>{{ showFeelsLike }}</strong>
        </p>
      </div>
      <div className="weather-info__item">
        <p>Wind speed:<br>
          <strong>{{ showWind }}</strong></p>
      </div>
      <div className="weather-info__item weather-info__item--country">
        <p>{{ showCountry }}</p>
      </div>
    </div>
    
    <div className="weather-detail-country" v-if="info != null">
      <h3>Detail of {{ showCountryName }}</h3>
      <div className="weather-detail-country__block">
        <div>
          <img :src="`${this.detail.flags.svg}`" :alt="`${this.detail.flags.alt}`" :title="`${this.detail.flags.alt}`" height="100px"> 
        </div>
        <div>
          <img :src="`${this.detail.coatOfArms.svg}`" alt="emblem" height="100px"> 
        </div>
        <div className="weather-detail-country__cur" v-for="(res, index) in showCountryCurrencies" :key="index">
          <div>
            {{ res.name }}<br>
            <span className="weather-detail-country__cur--bold">{{ res.symbol }}</span>
          </div>
        </div>
      </div>
      
    </div>
  </div>
</template>

<script>
import axios from 'axios'

  export default {
    data() {
      return {
        name: '',
        info: null,
        error: '',
        countryDetail: '',
        detail: null,
        resultCurrencies: ''
      }
    },
    computed: {
      showTemp() {
        return Math.round(this.info.main.temp)
      },
      showFeelsLike() {
        return Math.round(this.info.main.feels_like)
      },
      showWind() {
        return Math.round(this.info.wind.speed)
      },
      showCountry() {
        return `Country: ${this.info.sys.country}`
      },
      showCountryName() {
        return this.detail.name.common
      },
      showCountryCurrencies() {
        this.resultCurrencies = this.detail.currencies
        return this.resultCurrencies
      },
    },
    methods: {
      getWearher() {
        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.name}&units=metric&appid=8185f81c4e330cf9cde260940aba7065`)
        .then(res => (this.info = res.data));

        if(this.info == null) {
          return this.error = "ERROR: Undefind city";
        } else { 
          this.error = "" 
        }
        this.countryDetail = this.info.sys.country;
        axios.get(`https://restcountries.com/v3.1/alpha/${this.countryDetail}`)
        .then(det => (this.detail = det.data[0]));
      }
    }
  }
</script>

<style scoped>

.weather {
  background-color: #FFEDDF;
  width: 80vw;
  min-height: 400px;
  margin: 8vh auto;
  padding: 40px;  
  color: #2d3047;
  text-align: center;
  border-radius: 5px;
  font-family: Georgia, 'Times New Roman', Times, serif;
}
.weather-search {
  display: flex;
  justify-content: center;
  gap: 10px;
  padding-block: 20px 10px;
}
button {
  border: none;
  padding: 10px 15px;
  border-radius: 5px;
  background-color: #2d3047;
  border: 2px solid #2d3047;
  color: #FFEDDF;
  font-weight: bold;
  cursor: pointer;
}
button:hover {
  background-color: #FFEDDF;
  color: #2d3047;
  border: 2px solid #2d3047;
}
input {
  background-color: transparent;
  border: 0px;
  border: 2px solid #FFEDDF;
  border-bottom: 2px solid #2d3047;
  cursor: pointer;
  padding: 5px;
  width: 20vw;
}
input:hover {
  border: 2px solid white;
  border-radius: 5px;
}
input:focus {
  outline: none;
  border-radius: 5px;
  border: 2px solid #2d3047;
}
input[placeholder] {
  color: #94bbe9;
  font-weight: bold;
  font-size: 24px;
}

.weather-info {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}
.weather-info__item {
  background-color: #2d3047;
  color: #FFEDDF;
  width: 150px;
  height: 150px;
  border-radius: 50%;
  font-size: 22px;
}
.weather-info__item--country {
  background-color: #94bbe9;
}
.weather-info__item:hover {
  background-color: #FFEDDF;
  color: #2d3047;
  outline: 2px solid #2d3047;
}
.weather-info__item p {
  padding-top: 56px;
}
.weather-detail-country {
  padding-top: 30px;
}
.weather-detail-country h3 {
  padding-bottom: 20px;
}
.weather-detail-country__block {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: 20px;
}
.weather-detail-country__cur {
  display: flex;
  flex-direction: column;
}
.weather-detail-country__cur--bold {
  font-size: 32px;
  font-weight: bold;
}
</style>
