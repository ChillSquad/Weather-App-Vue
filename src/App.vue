<script>
import axios from "axios";
export default {
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },
  computed: {
    showTemp() {
      return "Температура: " + this.info.main.temp;
    },
    showFeelsLike() {
      return "Ощущается как: " + this.info.main.feels_like;
    },
    minTemp() {
      return "Минимальная температура: " + this.info.main.temp_min;
    },
    maxTemp() {
      return "Максимальная температура: " + this.info.main.temp_max;
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Ошибка ввода";
        return false;
      }
      this.error = "";

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=50edd9f6cf13797a3c83bbac45438572`
        )
        .then((res) => (this.info = res.data));
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Прогноз погоды</h1>
    <h4>
      Узнать погоду в городе: <span class="capitalize">{{ city }}</span>
    </h4>
    <input type="text" v-model="city" placeholder="Введите город" />
    <button v-show="city != ''" @click="getWeather()">Найти</button>
    <p class="error" v-show="city != '' && error != ''">{{ error }}</p>

    <div class="info" v-if="info != null">
      <p>{{ showTemp }} ℃</p>
      <p>{{ showFeelsLike }} ℃</p>
      <p>{{ minTemp }} ℃</p>
      <p>{{ maxTemp }} ℃"</p>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: #1f0f24;
  color: #fff;
  padding: 20px;
  text-align: center;
}

.wrapper .info {
  margin-top: 30px;
  display: flex;
  flex-direction: column;
}

.active {
  font-size: 32px;
}

.info p {
  margin-top: 20px;
}

.wrapper h1 {
  margin-top: 30px;
}

.wrapper h4 {
  margin-top: 20px;
  margin-bottom: 10px;
}

.capitalize {
  display: inline-block;
  text-transform: lowercase;
}

.capitalize:first-letter {
  text-transform: capitalize;
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
  border-block-color: #6e2d7d;
}

.wrapper button {
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  translate: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}

.error {
  color: #d03939;
  margin-top: 20px;
}
</style>
