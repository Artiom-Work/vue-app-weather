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
    cityName() {
      return "«" + this.city + "»";
    },
    showTemp() {
      return "Температура: " + this.info.main.temp;
    },
    showFeelsLike() {
      return "Ощущается как: " + this.info.main.feels_like;
    },
    showMinTemp() {
      return "Минимальная температура: " + this.info.main.temp_min;
    },
    showMaxTemp() {
      return "Максимальная температура: " + this.info.main.temp_max;
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Нужно название более одного символа";
        return false;
      }

      this.error = "";

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=11a9e717b9215c7149234737a43f04ca`
        )
        .then((res) => (this.info = res.data));
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
    <input type="text" v-model="city" placeholder="Введите город" />
    <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
    <button disabled v-else>Введите название города</button>
    <p class="error">{{ error }}</p>

    <div v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<style scoped>
.error {
  color: #d03939;
  font-size: 16px !important;
}

.wrapper {
  width: 600px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: #1f0f24;
  text-align: center;
  font-family: sans-serif;
  color: #fff;
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
  background: #e3bc5b;
  color: #fff;
  border-radius: 7px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  transition: transform 500ms ease;
  cursor: pointer;
}

@media (hover: hover) {
  .wrapper button:not(:disabled):hover {
    transform: scale(1.1) translateY(-5px);
  }
}
.wrapper button:not(:disabled):active {
  transform: scale(0.95);
  filter: brightness(80%);
}
@media (max-width: 767px) {
  .wrapper {
    max-width: 320px;
    width: 100%;
    box-sizing: border-box;
  }

  .wrapper h1 {
    font-size: 24px;
  }

  .wrapper input {
    margin-bottom: 20px;
    float: unset;
  }

  .wrapper button {
    margin-left: unset;
    display: block;
    margin-inline: auto;
  }
}
</style>
