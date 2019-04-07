<template>
  <div class="row justify-content-md-center">
    <div class="contenair col-xl-4 text-center">
      <h1>Weather of {{echoCity}}</h1>
        <div class="input-group pl-0 pr-0 col-xl-12 mb-3">
          	<input type="text" class="form-control" v-on:keyup.enter="getData" placeholder="Choose a city in the world [Only in English]" v-model="city">
			<div class="input-group-append">
				<button type="submit" v-on:click="getData" class="btn btn-secondary">
				<i class="fa fa-search"></i> Search
				</button>
			</div>
        </div>
		<transition name="fade" enter-active-class="animated bounceInDown">
			<div v-for="(d, index) in info" :key="d.id">
				<div v-if="index % 40===0">
					<div id="cadreHaut" >
						<div id="cadreImgWe">
						<img v-bind:src="'https://openweathermap.org/img/w/'+weather(d.weather[0].icon)" alt="icon météo" >
							<!-- <img v-bind:src="+weather(d.weather[0].icon)" style="width: 80%;" alt="icon météo" > -->
						</div>
						<div id="cadreImgtext">
							<div id="echoDate">
								{{formatdateDay(d.dt_txt)}} - {{formatdate(d.dt_txt)}}
							</div>
							<div id="echoTemp">
								<strong>{{d.main.temp}}</strong>°C
							</div>
							<div id="echoDesc">
								{{d.weather[0].description}}
							</div>
							<div id="cadGloAdd">
								<div class="echoAddinfo">
									<h6 class="float-left pr-3">
										<i class="wi wi-thermometer-exterior"></i>
									</h6>
									<span class="float-left text-muted">{{d.main.temp_min}}</span>
								</div>
								<div class="echoAddinfo">
									<h6 class="float-left pr-3">
										<i class="wi wi-thermometer"></i>
									</h6>
									<span class="float-left text-muted">{{d.main.temp_max}}</span>
								</div>
								<div class="echoAddinfo">
									<h6 class="float-left pr-3">
										<i class="wi wi-humidity"></i>
									</h6>
									<span class="float-left text-muted">{{d.main.humidity}}</span>
								</div>
								<div class="echoAddinfo">
									<h6 class="float-left pr-3">
										<i class="wi wi-strong-wind"></i>
									</h6>
									<span class="float-left text-muted">{{d.wind.speed}}</span>
								</div>
							</div>
						</div>
					</div>
				</div>
			</div>
		</transition>
		<div id="cadreMiddle">
			<div v-for="(d, index) in info" :key="d.id">
				<div id="echoTimeByTime" v-if="index < 8">
					<div id="echoTimeSec">
						{{formatdate(d.dt_txt)}}
					</div>
					<div id="echoWeather">
						<img v-bind:src="'https://openweathermap.org/img/w/'+weather(d.weather[0].icon)" alt="icon météo" >
					</div>
					<div id="echoTempSec">
						{{d.main.temp}}°C
					</div>
				</div>
			</div>
		</div>
		<div id="cadreBottom">
			<div v-for="(d, index) in info" :key="d.id">
				<div id="echoTimeByTime" v-if="index  %8 === 0">
					<div id="echoTimeSec">
						{{formatdateWeek(d.dt_txt)}}
					</div>
					<div id="echoWeather">
						<img v-bind:src="'https://openweathermap.org/img/w/'+weather(d.weather[0].icon)" alt="icon météo" >
					</div>
					<div id="echoTempSec">
						{{d.main.temp}}°C
					</div>
				</div>
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
      return moment(String(value)).format("dddd DD MMMM YYYY");
    },
    weather: function(value) {
      return value+'.png';
    },
    getData: function() {
      if (this.city) {
        axios
          .get(
            "https://api.openweathermap.org/data/2.5/forecast?q="+this.city +",belgium&units=metric&APPID=75d6cb34749dd0617eb6e06e44317d0b"
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
	border-radius: 0px;
	border: 1px solid transparent;
}

.cadWeek {
	width: 100%;
	border-top: 1px solid rgb(228, 228, 228);
}
.temp {
	width: 50%;
	text-align: center;
	font-size: 70px;
	float: left;
	font-weight: 100;
}
.description {
	width: 50%;
	text-align: center;
	float: left;
	font-weight: 100;
  	font-size: 25px;
  	color: rgb(148, 147, 147);
}
.text-central {
  	text-align: center;
}
#cadreHaut, #cadreMiddle, #cadreBottom{
	width: 100%;
	padding: 15px;
	font-weight: normal;
	display: flex;
}
#cadreMiddle{
	margin-bottom: 20px;

}
#cadreBottom{

}
#cadreImgWe{
	width: 50%;
	border-radius: 3px;
	justify-content: left
}
#cadreImgWe img{
	width: 100%;
}
#cadreImgtext{
	width: 50%;
	border-radius: 3px;
	justify-content: right;
}
#echoDate, #echoTemp{
	text-align: center;
	padding: 10px 0;
	color: rgb(128, 128, 128);

}
#echoTemp{
	font-size: 70px;
	font-weight: 100;
}
#echoDesc{
	font-size: 20px;
	color: rgb(122, 122, 122)
}
#cadGloAdd{
	display: flex;
	padding: 10px 0;
}
.echoAddinfo{
	width: 50%;
	font-size: 15px;
	color: rgb(122, 122, 122);
	justify-content: center;
	padding: 3px;
}
#echoTimeByTime{
	width: auto;
	padding: 10px;
	justify-content: center;
	font-size: 13px;

}
#echoTimeSec, #echoWeather, #echoTempSec{
	height: auto;
	font-weight: bold;	
	color:rgb(139, 139, 139)
}
#echoWeather{

}
#echoTempSec{
	font-weight: 100;
	font-size: 12px;
	color: rgb(107, 106, 106)

}
</style>












