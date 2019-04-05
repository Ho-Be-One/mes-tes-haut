<template>
  <div class="row justify-content-md-center">
    <div class="contenair col-xl-5 text-center">
      <h1>Weather of {{echoCity}}</h1>
      <div class="form-row col-xl-12">
        <div class="input-group ol-xl-2 mb-3">
          <input
            type="text"
            class="form-control"
            v-on:keyup.enter="getData"
            placeholder="Choose a city in the world [Only in English]"
            v-model="city"
          >
          <div class="input-group-append">
            <button type="submit" v-on:click="getData" class="btn btn-secondary">
              <strong>
                <i class="fa fa-search"></i> Search
              </strong>
            </button>
          </div>
        </div>
        <div class="list-group-item mb-3 border-0 p-0 col-xl-12">
          <ul
            class="list-group mb-3 col-xl-12 p-0"
            v-for="(d, index) in info"
            v-if="index % 40===0"
            :key="d.id"
          >
            <li class="list-group-item text-center">
              <h6 class="my-0 text-center">
                <strong>
                  <i class="fa fa-calendar"></i>
                  {{formatdateDay(d.dt_txt)}}
                  <i class="fa fa-clock-o"></i>
                  {{formatdate(d.dt_txt)}}
                </strong>
              </h6>
            </li>

            <li class="list-group-item text-center pt-3 pb-0 pl-0 pr-0">
              <span class="float-left col-xl-6">
                <img
                  v-bind:src="'https://openweathermap.org/img/w/'+weather(d.weather[0].icon)"
                  style="width: 80%;"
                  alt="icon météo"
                >
              </span>
              <span class="float-left col-xl-6 pt-2 pb-2">
                <h1>
                  <span class="text-muted">
                    <strong>{{d.main.temp}}</strong>°C
                  </span>
                </h1>
                <span class="text-muted">{{d.weather[0].description}}</span>
              </span>
              <span class="float-left col-xl-6 pt-2 pb-2 pr-5 pl-5">
                <div class="float-left col-xl-6 pt-2 pb-2 pr-1 pl-4">
                  <h6 class="my-0 float-left pr-3">
                    <i class="wi wi-thermometer-exterior"></i>
                  </h6>
                  <span class="float-left text-muted">{{d.main.temp_min}}</span>
                </div>
                <div class="float-left col-xl-6 pt-2 pb-2 pr-1 pl-4">
                  <h6 class="my-0 float-left pr-3">
                    <i class="wi wi-thermometer"></i>
                  </h6>
                  <span class="float-left text-muted">{{d.main.temp_max}}</span>
                </div>

                <div class="float-left col-xl-6 pt-2 pb-2 pr-1 pl-4">
                  <h6 class="my-0 float-left pr-3">
                    <i class="wi wi-humidity"></i>
                  </h6>
                  <span class="float-left text-muted">{{d.main.humidity}}</span>
                </div>
                <div class="float-left col-xl-6 pt-2 pb-2 pr-1 pl-4">
                  <h6 class="my-0 float-left pr-3">
                    <i class="wi wi-strong-wind"></i>
                  </h6>
                  <span class="float-left text-muted">{{d.wind.speed}}</span>
                </div>
              </span>
            </li>
          </ul>
        </div>

        <div class="p-0 col-xl-12">
          <ul
            class="col-xl-3 float-left p-0"
            v-for="(d, index) in info"
            v-if="index < 4"
            :key="d.id"
            >
            <li
              class="line-bottom list-group-item text-center p-0"
              style="width: 100%; margin: 0 auto;"
            >
              <div class="p-0 col-xl-6 float-left">
                <span class="text-muted">
                  <img
                    v-bind:src="'https://openweathermap.org/img/w/'+weather(d.weather[0].icon)"
                    alt="icon météo"
                  >
                </span>
              </div>

              <div class="p-0 col-xl-6 float-left">
                <h6 class="text-muted">
                  <i class="fa fa-clock-o"></i>
                  {{formatdate(d.dt_txt)}}
                </h6>
                <span class="text-muted">
                  <strong>{{d.main.temp}}</strong>°C
                </span>
              </div>
            </li>
          </ul>
        </div>
        <div class="cadWeek">
          <ul
            class="list-group float-left p-0 m-0 col-xll-3"
            v-for="(d, index) in info"
            v-if="index % 8===0"
            :key="d.id"
          >
            <li class="weekDate list-group-item">
              <h6 class="my-0 text-center">{{formatdateWeek(d.dt_txt)}}</h6>
              <hr>
              <div class="p-0 text-center col-xl-6">
                <img
                  style="width: 100%;"
                  v-bind:src="'https://openweathermap.org/img/w/'+weather(d.weather[0].icon)"
                  alt="icon météo"
                >
              </div>

              <div class="p-0 col-xl-6">
                <span class="text-muted">
                  <strong>{{d.main.temp}}</strong>°C
                </span>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";

export default {
  data() {
    return {
      info: "",
      city: "",
      echoCity: "Ville",
      cityvalue: false
    };
  },
  methods: {
    formatdate: function(value) {
      return moment(String(value)).format("HH:mm");
    },
    formatdateWeek: function(value) {
      return moment(String(value)).format("ddd DD ");
    },
    formatdateDay: function(value) {
      return moment(String(value)).format("dddd d MMMM YYYY");
    },
    weather: function(value) {
      return value + ".png";
    },
    getData: function() {
      if (this.city) {
        axios
          .get(
            "https://api.openweathermap.org/data/2.5/forecast?q=" +
              this.city +
              "&units=metric&APPID=75d6cb34749dd0617eb6e06e44317d0b"
          )
          .then(response => (this.info = response.data.list));

        this.echoCity = this.city.charAt(0).toUpperCase() + this.city.slice(1);

        this.city = "";
        this.info = "";
      } else {
        alert("Enter a city");
      }
    }
  }
};
</script>

<style>
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.0/animate.min.css");
.col-xll-3 {
  width: 20%;
}
.bground {
  background-color: rgb(229, 231, 235);
  color: rgb(67, 70, 82);
}
.line-bottom {
  border: 1px solid transparent;
  float: left;
  border-radius: 0;
}
.line-bottom h6 {
  margin: 0;
}
.weekDate {
  border-radius: 0px 0px 0px 0px;
  border: 1px solid transparent;
}
.cadWeek {
  width: 100%;
  float: left;
  border-top: 1px solid rgb(228, 228, 228);
}
</style>












