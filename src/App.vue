<template>
  <div id="app">
    <main>
      <input type="text" class="search-box" placeholder="Search..." v-on:keyup.enter="fetchWeather()" v-model="query" />
      <div>
        <div class="location">{{ city }}</div>
        <div class="date" v-if="temperature">{{ date }}</div>
        <div class="temperaturewrapper" v-if="temperature">
          <div class="temperature">{{ temperature }}</div>
          <div class="condition">{{ condition }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import moment from "moment";
export default {
  name: "App",
  components: {},
  data() {
    return {
      query: "",
      api_key: process.env.VUE_APP_API_KEY,
      temperature: "",
      city: "",
      condition: "",
      date: "",
    };
  },
  methods: {
    fetchWeather() {
      this.$axios.get("https://api.openweathermap.org/data/2.5/weather?q=" + this.query + "&appid=" + this.api_key).then((data) => {
        data = data.data;
        console.log(data);
        this.temperature = Math.round((data.main.temp - 273.15) * 10) / 10 + " °C";
        this.city = data.name + ", " + data.sys.country;
        this.condition = data.weather[0].description;
        //TODO: get utc and calc time in location requested
        this.date = moment().utcOffset(data.timezone);
      });
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: "Montserrat", sans-serif;
  display: flex;
  flex-flow: column;
  align-items: center;
  background: url("assets/bg.jpg");
  overflow: hidden;
}
main {
  min-height: 100vh;
  min-width: 100vw;
  padding: 40px;
  background-image: linear-gradient(rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0.9));
}
.search-box {
  margin: 20px 0px;
  width: 100%;
  box-sizing: border-box;
  font-size: 20px;
  padding: 10px;
  box-shadow: rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 12px 0px 12px;
  appearance: none;
  outline: none;
  transition: all 0.2s;
}
.search-box:focus {
  border-radius: 0;
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 12px 0px 12px 0px;
}

.location {
  text-align: center;
  font-size: 40px;
  font-style: italic;
  color: #fff;
  text-shadow: 2px 1px #000;
  font-weight: 600;
}

.temperaturewrapper {
  background-color: rgba(255, 255, 255, 0.5);
  text-align: center;
  margin: 25px 80px;
  border-radius: 5px;
  padding: 5px;
  font-size: 30px;
}

.temperature {
  margin: 0px 12px;
}

.condition {
  text-transform: capitalize;
}
</style>
