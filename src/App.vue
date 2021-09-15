<!-- @format -->

<template>
	<div
		id="app"
		:class="
			typeof result.main != 'undefined' && result.main.temp > 16 ? 'warm' : ''
		"
	>
		<main>
			<div class="search-box">
				<input
					type="text"
					class="search-bar"
					placeholder="Search..."
					v-model="cityname"
					@keypress="searchWeatherData"
				/>
			</div>
			<div class="weather-wrap" v-if="typeof result.main != 'undefined'">
				<div class="location-box">
					<div class="location">
						{{ result.name }}, {{ result.sys.country }}
					</div>
					<div class="date">{{ dateBuilder() }}</div>
				</div>

				<div class="weather-box">
					<div class="temp">{{ Math.round(result.main.temp) }} °c</div>
					<div class="weather">{{ result.weather[0].main }}</div>
				</div>
			</div>
		</main>
	</div>
</template>

<script>
	import axios from "axios";
	export default {
		name: "App",
		data() {
			return {
				api_key: "20c21ee4256a556a18a6688d69a984cc",
				cityname: "Kolkata",
				result: {},
			};
		},
		created() {
			axios
				.get(
					`https://api.openweathermap.org/data/2.5/weather?q=${this.cityname}&units=metric&appid=${this.api_key}`
				)
				.then((res) => {
					this.result = res.data;
				})
				.catch((e) => console.log(e));
		},
		methods: {
			searchWeatherData(e) {
				if (e.key == "Enter") {
					console.log(this.cityname);
					console.log(this.api_key);
					axios
						.get(
							`https://api.openweathermap.org/data/2.5/weather?q=${this.cityname}&units=metric&appid=${this.api_key}`
						)
						.then((res) => {
							console.log(res.data);
							this.result = res.data;
						})
						.catch((e) => console.log(e));
				}
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
	};
</script>

<style>
	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}
	body {
		font-family: "montserrat", sans-serif;
	}
	#app {
		background-image: url("./assets/cold-bg.jpg");
		background-size: cover;
		background-position: bottom;
		transition: 0.4s;
	}
	#app.warm {
		background-image: url("./assets/warm-bg.jpg");
	}
	main {
		min-height: 100vh;
		padding: 25px;
		background-image: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.25),
			rgba(0, 0, 0, 0.75)
		);
	}

	.search-box {
		width: 100%;
		margin-bottom: 30px;
	}
	.search-box .search-bar {
		display: block;
		width: 100%;
		padding: 15px;

		color: #313131;
		font-size: 20px;
		appearance: none;
		border: none;
		outline: none;
		background: none;
		box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
		background-color: rgba(255, 255, 255, 0.5);
		border-radius: 0px 16px 0px 16px;
		transition: 0.4s;
	}
	.search-box .search-bar:focus {
		box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
		background-color: rgba(255, 255, 255, 0.75);
		border-radius: 16px 0px 16px 0px;
	}
	.location-box .location {
		color: #fff;
		font-size: 32px;
		font-weight: 500;
		text-align: center;
		text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
	}
	.location-box .date {
		color: #fff;
		font-size: 20px;
		font-weight: 300;
		font-style: italic;
		text-align: center;
	}
	.weather-box {
		text-align: center;
	}
	.weather-box .temp {
		display: inline-block;
		padding: 10px 25px;
		color: #fff;
		font-size: 102px;
		font-weight: 900;
		text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
		background-color: rgba(255, 255, 255, 0.25);
		border-radius: 16px;
		margin: 30px 0px;
		box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
	}
	.weather-box .weather {
		color: #fff;
		font-size: 48px;
		font-weight: 700;
		font-style: italic;
		text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
	}
</style>
