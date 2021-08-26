<template>
	<Header />
	<Menu @form="handleFormResult" />
	<div class="container" v-if="data[0]">
		<Chart
			v-for="type in config.types"
			:key="type.label"
			class="chart"
			:width="500"
			:height="400"
			:data="data"
			:type="type"
		/>
	</div>
</template>

<script>
import axios from "axios";

import Header from "./components/TheHeader.vue";
import Menu from "./components/TheMenu.vue";
import Chart from "./components/TheChart.vue";

export default {
	name: "App",
	components: {
		Header,
		Menu,
		Chart,
	},
	data() {
		return {
			config: {},
			data: [],
			countries: [],
		};
	},
	watch: {
		"config.country": {
			handler() {
				this.fetchCovidData();
			},
		},
	},
	methods: {
		async fetchCovidData() {
			if (this.config.country) {
				const url = `https://corona-api.com/countries/${this.config.country}?include=timeline`;
				const response = (await axios(url)).data;
				const data = response.data.timeline;
				this.data.push({
					country: this.config.country,
					data,
				});
			}
		},
		handleFormResult(value) {
			this.config = value;
		},
	},
	mounted() {},
};
</script>

<style>
	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
	}

	.chart {
		margin: 10px;
	}
</style>
