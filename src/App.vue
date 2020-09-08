<template>
  <div id="app">
    <div>
      <input type="text" name id class="search-box" placeholder="Search..." v-on:keyup.enter="fetchWeather()" v-model="query" />
    </div>
    <div>
      <div class="location">{{ city }}</div>
      <div class="temperature">{{ temperature }}</div>
      <div class="forcast">{{ condition }}</div>
    </div>
  </div>
</template>

<script>
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
    };
  },
  methods: {
    fetchWeather() {
      this.$axios.get("https://api.openweathermap.org/data/2.5/weather?q=" + this.query + "&appid=" + this.api_key).then((data) => {
        data = data.data;
        this.temperature = Math.round((data.main.temp - 273.15) * 10) / 10 + " °C";
        this.city = data.name + ", " + data.sys.country;
        this.condition = data.weather[0].description;
      });
    },
  },
};
</script>
M
<style lang="sass" scoped></style>>
