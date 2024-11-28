<script lang="ts">
export default {
  data() {
    return {
      city: '',
    }
  },
  methods: {
    clearInput() {
      this.city = ''
    },
  },
  computed: {
    cityName() {
      return `«${this.city}»`
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

    <button class="btn-send" v-show="city != ''">Запросить погоду</button>
  </div>
</template>

<style scoped>
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
</style>
