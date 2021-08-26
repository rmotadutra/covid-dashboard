<template>
	<div class="container">
		<form action="">
			<DropDown
				id="country"
				name="country"
				label="Selecione um país..."
				:options="countries"
				:onChange="handleInputChange"
			/>
			<CheckBox
				v-for="type in types"
				:key="type.label"
				v-model="type.checked"
				:label="type.label"
			/>
		</form>
	</div>
</template>

<script>
import axios from "axios";

import CheckBox from "./CheckBox.vue";
import DropDown from "./DropDown.vue";

export default {
	components: {
		DropDown,
		CheckBox,
	},
	data() {
		return {
			countries: [],
			countriesSelected: [],
			types: [
				{
					label: "Cases",
					value: "confirmed",
					checked: false,
				},
				{
					label: "Deaths",
					value: "deaths",
					checked: false,
				},
				{
					label: "New Deaths",
					value: "new_deaths",
					checked: false,
				},
			],
			formValues: {},
		};
	},
	watch: {
		formValues: {
			handler() {
				this.$emit("form", this.formValues);
			},
			deep: true,
		},
		types: {
			handler() {
				this.formValues = { ...this.formValues, types: this.types };
			},
			deep: true,
		},
	},
	methods: {
		async fetchCountries() {
			const url = `https://corona-api.com/countries`;
			const response = (await axios(url)).data;
			const data = response.data;
			this.countries = this.parseCountries(data);
		},
		parseCountries(countries) {
			return countries
				.map((country) => ({
					label: country.name,
					value: country.code,
				}))
				.sort(this.sortByLabelAscending);
		},
		sortByLabelAscending(a, b) {
			return a.label.localeCompare(b.label);
		},
		handleInputChange(event) {
			const { value, name } = event.target;
			this.formValues = { ...this.formValues, [name]: value };
		},
	},
	mounted() {
		this.fetchCountries();
	},
};
</script>

<style scoped>
	.container {
		width: 100%;
	}
	.container form {
		display: flex;
		align-items: center;
		justify-content: flex-start;
	}
	.container form select {
		margin-left: 20px;
	}
</style>