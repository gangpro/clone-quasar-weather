<template>
  <q-page class="flex column" :class="bgClass">
    <!-- 검색창 영역 -->
    <div class="col q-pt-lg q-px-md">
      <!-- https://quasar.dev/vue-components/input -->
      <q-input
        v-model="search"
        @keyup.enter="getWetherBySearch"
        placeholder="검색"
        dark
        boarderless
        >
        <template v-slot:before>
          <q-icon
            @click="getLocation"
            name="my_location"
          />
        </template>

        <template v-slot:append>
          <q-btn
            @click="getWetherBySearch"
            round
            dense
            flat
            icon="search"
          />
        </template>
      </q-input>
    </div>
    <!-- 날씨 영역 -->
    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h6 text-weight-light">
          {{ weatherData.sys.country }}
        </div>
        <div class="text-h4 text-weight-light">
          {{ weatherData.name }}
        </div>
        <div class="text-h6 text-weight-light">
          {{ weatherData.weather[0].main }}
        </div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>{{ parseInt(weatherData.main.temp) }}</span>
          <span class="text-h4 relative-position degree">&deg;C</span>
        </div>
      </div>
      <div class="col text-center">
        <img :src="`http://openweathermap.org/img/wn/${ weatherData.weather[0].icon }@2x.png`" >
      </div>
    </template>
    <!-- 현재위치 영역 -->
    <template v-else>
      <div class="col column text-center text-white">
        <div class="col text-h2 text-weight-thin">
          Quasar<br>Weather
        </div>
        <q-btn
          @click="getLocation"
          class="col"
          flat>
          <q-icon left size="3em" name="my_location" />
          <div>내 현재 위치</div>
        </q-btn>
      </div>
    </template>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage',
  data() {
    return {
      search: '',
      weatherData: null,
      lat: null,
      lon: null,
      apiUrl: "https://api.openweathermap.org/data/2.5/weather",
      apiKey: ""
    }
  },
  computed: {
    bgClass() {
      if(this.weatherData) {
        //this.weatherData.weather[0].icon.endsWith("n")? "bg-night" : "bg-day"
        if(this.weatherData.weather[0].icon.endsWith("n")) {
          return "bg-night"
        }
        else {
          return "bg-dat"
        }
      }
    }
  },
  methods: {
    getLocation() {
      this.$q.loading.show()

      if(this.$q.platform.is.electron) {

        this.$axios.get(`https://freegeoip.app/json/`).then(res => {
          this.lat = res.data.latitude
          this.lon = res.data.longitude
          this.getWeatherByCoords()
        },
        err => {
          console.log("[ERROR] : ", err.message);
          this.textContent = err.message;
        })

      }
      else {

        //console.log("getLocation >>> ")
        navigator.geolocation.getCurrentPosition(position => {
          // console.log("위치정보 : ", position);
          this.lat = position.coords.latitude
          this.lon = position.coords.longitude
          this.getWeatherByCoords()
        },
        err => {
          console.log("[ERROR] : ", err.message);
          this.textContent = err.message;
        })

      }

    },
    getWeatherByCoords() {
      this.$q.loading.show()
      //console.log("getWeatherByCoords >>> ")

      this.$axios(`${this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`).then(res => {
        // console.log("응답 : ", res)
        this.weatherData = res.data
        this.$q.loading.hide()
      },
      err => {
        console.log("[ERROR] : ", err.message);
        this.textContent = err.message;
        this.$q.loading.hide()
      })
    },
    getWetherBySearch() {
      this.$q.loading.show()
      //console.log("getWetherBySearch >>> ")

      this.$axios(`${this.apiUrl}?q=${this.search}&appid=${this.apiKey}&units=metric`).then(res => {
        // console.log("응답 : ", res)
        this.weatherData = res.data
        this.$q.loading.hide()
      },
      err => {
        console.log("[ERROR] : ", err.message);
        this.textContent = err.message;
        this.$q.loading.hide()
      })
    },
  }
})
</script>

<!-- sass : https://uigradients.com -->
<style lang="sass">
  .q-page
    background: linear-gradient(to top, #74ebd5, #acb6e5)
    &.bg-night
      background: linear-gradient(to top, #232526, #414345)
    &.bg-day
      background: linear-gradient(to top, #00b4db, #0083b0)
  .degree
    top: -44px
</style>
