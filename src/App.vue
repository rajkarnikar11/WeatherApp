<template>
	<div
		id="app"
		:class="
			typeof weather.main != 'undefined' && weather.main.temp > 16
				? 'warm'
				: ' '
		"
	>
		<main>
			<h1 class="title">Weather App</h1>
			<div class="search-box">
				<input
					type="text"
					name=""
					id=""
					class="search-bar"
					placeholder="Enter City"
					v-model="query"
					@keypress="fetchWeather"
				/>
				<div
					class="weather-wrap"
					v-if="typeof weather.main != 'undefined'"
				>
					<div class="location-box">
						<div class="location">
							{{ weather.name }} , {{ weather.sys.country }}
						</div>
						<div class="date">{{ dateBuilder() }}</div>
					</div>
					<div class="weather_box">
						<div class="temp">
							{{ Math.round(weather.main.temp) }}°c
						</div>
						<div class="weather">{{ weather.weather[0].main }}</div>
					</div>
				</div>
			</div>
		</main>
	</div>
</template>

<script>
export default {
	name: "App",
	data() {
		return {
			api_key: "d3ecb3f2976ed0c5c03f11451504528b",
			url_base: "http://api.openweathermap.org/data/2.5/",
			query: " ",
			weather: {},
		};
	},
	methods: {
		fetchWeather(e) {
			if (e.key == "Enter") {
				fetch(
					`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`,
				)
					.then((res) => {
						return res.json();
					})
					.then(this.setResults);
			}
		},
		setResults(results) {
			this.weather = results;
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
			return `${day} ${month} ${year}`;
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
	/* font-family: Avenir, Helvetica, Arial, sans-serif; */
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	font-family: montserrat;

	background-image: url("./assets/cold.jpg");
	background-size: cover;
	background-position: top;
	transition: 0.4s;
}
#app.warm {
	background-image: url("./assets/warm.jpg");
}
main {
	min-height: 100vh;
	padding: 25px;
	background-image: linear-gradient(
		to bottom,
		rgb(0, 0, 0, 0.25),
		rgb(0, 0, 0, 0.75)
	);
}
.title {
	color: rgb(255, 255, 255, 0.75);
	font-size: 50px;
	margin: 30px;
	text-transform: capitalize;
	text-shadow: 3px 3px 2px rgba(0, 0, 0, 0.25);
}
.weather-wrap {
	/* outline: 1px solid red; */
	margin-top: 50px;
}
.location {
	color: white;
	font-size: 32px;
	font-weight: 500;
	text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.date {
	color: white;
	font-weight: 300;
	font-size: 20px;
	margin-top: 20px;
	font-style: italic;
	text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.search-box .search-bar {
	display: block;
	width: 100%;
	padding: 15px;
	font-size: 20px;
	appearance: none;
	border: none;
	outline: none;
	background: none;
	background-color: rgba(255, 255, 255, 0.5);
	border-radius: 0px 16px 0px 16px;
	box-shadow: 0px 0px 8px rgba(255, 255, 255, 0.25);
	transition: 0.4s;
}
.search-box .search-bar:focus {
	background-color: rgba(255, 255, 255, 0.75);
	box-shadow: 0px 0px 8px rgba(255, 255, 255, 0.75);
	border-radius: 16px 0px 16px 0px;
}

.temp {
	display: inline-block;
	padding: 10px 25px;
	color: white;
	font-size: 105px;
	font-weight: 900;
	text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
	border-radius: 16px;
	background-color: rgba(255, 255, 255, 0.25);
	margin: 30px 0;
	box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather {
	color: white;
	font-size: 48px;
	font-weight: 700;
	font-style: italic;
	text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
