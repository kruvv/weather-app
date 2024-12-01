<script lang="ts">
import axios from 'axios'

export default {
  data() {
    return {
      city: '',
      error: '',
      units: 'metric', //imperial | metric
      lang: 'ru',
      info: null,
      symbolUnit: '℃ ',
      axiosError: {},
    }
  },
  watch: {
    units(newUnits, oldUnits) {
      if (newUnits != oldUnits) {
        this.getWeather()
        this.selectUnits()
      }
    },
  },
  methods: {
    clearInput() {
      this.city = ''
      this.info = null
    },
    getWeather() {
      // Проверяем запрос
      if (this.city.trim().length <= 2) {
        this.error = 'Для поиска необходимо ввести более 2-х символов'
        return false
      }

      this.error = ''

      // Делаем запрос к api
      const URL_API = `${import.meta.env.VITE_VUE_APP_API_URL}q=${this.city}&units=${this.units}&lang=${this.lang}&appid=${import.meta.env.VITE_VUE_APP_SECRET_KEY}`
      axios
        .get(URL_API)
        .then((res) => {
          this.info = res.data
        })
        .catch(function (err) {
          if (err.response) {
            console.error(err.response.data)
          }
        })
    },
    selectUnits() {
      return (this.symbolUnit = this.units == 'metric' ? '℃ ' : '°F')
    },
  },
  computed: {
    cityName() {
      return `«${this.city.slice(0, 1).toUpperCase()}${this.city.slice(1)}»`
    },

    showTemp() {
      return `Температура: ${Math.round(this.info.main.temp)} ${this.symbolUnit}`
    },

    showFeelsLike() {
      return `Ощущается как: ${Math.round(this.info.main.feels_like)} ${this.symbolUnit}`
    },

    showMinTemp() {
      return `Минимальная температура: ${Math.round(this.info.main.temp_min)} ${this.symbolUnit}`
    },

    showMaxTemp() {
      return `Максимальная температура: ${Math.round(this.info.main.temp_max)} ${this.symbolUnit}`
    },

    showHumidity() {
      return `Влажность: ${Math.round(this.info.main.humidity)}%`
    },

    showStatus() {
      return this.axiosError?.cod
    },

    showStatusText() {
      return this.axiosError?.message
    },
  },
}
</script>

<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == '' ? 'вашем городе' : cityName }}</p>
    <input type="text" v-model="city" placeholder="Введите город" />
    <button class="btn-clear" v-show="city != ''" @click="clearInput">x</button>

    <button class="btn-send" v-if="city != ''" @click="getWeather">Запросить погоду</button>
    <p class="msg-error">{{ error }}</p>

    <div class="info" v-if="info != null">
      <div class="units">
        <input type="radio" id="unitC" value="metric" v-model="units" name="units" checked />
        <label for="C">℃ </label>
        <input type="radio" id="unitF" value="imperial" v-model="units" name="units" />
        <label for="F">°F</label>
      </div>

      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
      <p>{{ showHumidity }}</p>
    </div>
    <div class="axios-error" v-else-if="!axiosError">
      <p>{{ showStatus }}</p>
      <p>{{ showStatusText }}</p>
    </div>
  </div>
</template>

<style scoped>
#unitC,
#unitF {
  margin: 5px 5px 5px 15px;
}

.msg-error {
  color: #f00;
  font-size: 1.4rem;
}

.info {
  margin-top: 40px;
}

.wrapper {
  position: relative;
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: #1f0f24;
  padding: 20px;
  text-align: center;
  color: #fff;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin: 30px;
  background: transparent;
  border: none;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 1.2rem;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: rgba(0, 150, 255, 0.5);
}

.wrapper .btn-send {
  padding: 15px 30px;
  background: rgb(14, 14, 26);
  color: #fff;
  font-weight: bold;
  border: none;
  border-radius: 5px;
  box-shadow: 0 0 15px rgba(0, 150, 255, 0.5);
  cursor: pointer;
  transition: all 0.3s ease;
  -webkit-box-reflect: below 10px linear-gradient(to bottom, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.4));
}

.wrapper .btn-send:hover {
  background: linear-gradient(134deg, #3498db, #3ecc73);
}

.wrapper .btn-clear {
  position: absolute;
  background: transparent;
  color: #fff;
  top: 34%;
  left: 480px;
  width: 20px;
  border-radius: 50%;
}

.btn-clear:hover {
  cursor: pointer;
}
</style>
