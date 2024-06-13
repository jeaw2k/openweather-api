<template>
  <div class="wrapper">
    <h1>Погода</h1>
    <p>Дізнатись погоду у {{ city == "" ? "вашому місті" : cityName  }}</p>
    <input type="text" v-model="city" placeholder="Вкажіть місто">
    <button v-if="city !=''" @click="getWeather()">Дізнатись погоду</button>
    <button disabled v-else>Вкажіть назву міста</button>
    <p class="error">{{ error }}</p>

    <div v-if="info !=null">
      <p> {{ showTemp }}</p>
      <p> {{ showFeelsLike }}</p>
      <p> {{ showMinTemp }}</p>
      <p> {{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return "'" + this.city + "'"
    },
    showTemp() {
      return "Температура: " + this.info.main.temp
    },
    showFeelsLike() {
      return "Відчувається як: " + this.info.main.feels_like
    },
    showMinTemp() {
      return "Мінімальна температура: " + this.info.main.temp_min
    },
    showMaxTemp() {
      return "Максимальна температура: " + this.info.main.temp_max
    },
  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = "Введіть більше чим 1 символ"
        return false
      }

      this.error = ""

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=a0a8094e82c812ec763bd1f62bcb5d43`)
          .then(res => (this.info = res.data))
    }
  }
}
</script>

<style scoped>
.error {
  color: #d03939
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: #1f0f24;
  text-align: center;
  color: #ffff;
}

.wrapper h1 {
 margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button:disabled {
  background: #746027;
  cursor: not-allowed;
}

.wrapper button {
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}
</style>
