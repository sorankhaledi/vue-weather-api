<template>
  <v-app>
    <v-container :class="setClass()">
      <div class="main px-15 py-5">
        <v-text-field
          class="px-5 searchbar"
          label="Search"
          v-model="city"
          @keypress="fetchData"
        ></v-text-field>
        <div class="information text-center my-15">
          <div class="location">{{ data.name }}, {{ data.sys.country }}</div>
          <div class="date h6 font-weight-regular">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-temp text-center">
          <div class="temp white--text">
            {{ Math.round(data.main.temp - 273.15) }}°C
          </div>
          <div class="weather mt-5">{{ data.weather[0].main }}</div>
        </div>
      </div>
    </v-container>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      data: {},
      api_key: "0a54443f47461f28d07a6674da213192",
      api_url: "http://api.openweathermap.org/data/2.5/weather?q=",
      city: "paveh",
    };
  },
  methods: {
    fetchData(event) {
      if (event.key == "Enter") {
        axios
          .get(`${this.api_url}${this.city}&appid=${this.api_key}`)
          .then((res) => {
            this.data = res.data;
            console.log(res.data.main);
          });
      }
    },
    setClass() {
      return this.data.main.temp -273.15 > 16 ? 'bg-warm' : '';
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
  mounted() {
    this.fetchData({ key: "Enter" });
  },
};
</script>


<style scoped>
.container {
  background: url(./assets/cold-bg.jpg) no-repeat;
  background-size: cover;
  background-position: bottom;
  min-height: 100vh;
  max-width: 600px;
  padding: 0;
  transition: 0.4s;
}

.bg-warm {
  background: url(./assets/warm-bg.jpg) no-repeat;
  background-size: cover;
  background-position: bottom;
}

.main {
  min-height: 100vh;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.74)
  );
}

.searchbar {
  background-color: rgba(255, 255, 255, 0.7);
  border-radius: 10px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.25);
}

.information .location {
  color: #fff;
  font-size: 32px;
  font-weight: 600;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.information .date {
  color: #fff;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.weather-temp .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 100px;
  font-weight: 900;

  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 10px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.25);
}

.weather-temp .weather {
  color: #fff;
  font-size: 60px;
  font-weight: 800;
}
</style>
