<template>
	<svg class="chart" :width="width" :height="height" v-if="type.checked">
		<g>
			<Axis
				:x="axisOffset"
				:y="0"
				:width="width"
				:height="height"
				:xlim="xlim"
				:ylim="ylim"
				:ylabel="type.label"
			/>
		</g>
		<g>
			<Grid
				:x="axisOffset"
				:y="0"
				:width="width - axisOffset"
				:height="height - axisOffset"
				:xlim="xlim"
				:ylim="ylim"
			/>
		</g>
		<g v-for="(item, index) in data" :key="item.country">
			<Line
				:x="axisOffset"
				:y="0"
				:width="width - axisOffset"
				:height="height - axisOffset"
				:data="item.data"
				:type="type.value"
				:xlim="xlim"
				:ylim="ylim"
				:color="getColor(index)"
			/>
		</g>
	</svg>
</template>

<script>
import { schemeCategory10 } from "d3-scale-chromatic";
import { min, max } from "d3-array";

import Line from "@/components/Line.vue";
import Axis from "@/components/Axis.vue";
import Grid from "@/components/Grid.vue";

export default {
	components: {
		Line,
		Axis,
		Grid,
	},
	props: {
		width: {
			type: Number,
			required: true,
		},
		height: {
			type: Number,
			required: true,
		},
		data: {
			type: Array,
			required: true,
		},
		type: {
			type: Object,
			required: true,
		},
	},
	data() {
		return {
			axisOffset: 100,
		};
	},
	computed: {
		xlim() {
			let minimo = [];
			let maximo = [];
			this.data.forEach((data) => {
				minimo.push(min(data.data, (d) => d.date));
				maximo.push(max(data.data, (d) => d.date));
			});
			const range = [min(minimo), max(maximo)];
			return range;
		},
		ylim() {
			console.log(this.type);
			let minimo = [];
			let maximo = [];
			this.data.forEach((data) => {
				minimo.push(min(data.data, (d) => d[this.type.value]));
				maximo.push(max(data.data, (d) => d[this.type.value]));
			});
			const range = [min(minimo), max(maximo)];
			return range;
		},
	},
	methods: {
		getColor(value) {
			if (value <= 9 && value >= 0) {
				return schemeCategory10[value];
			} else {
				return "#1e1e1e";
			}
		},
	},
};
</script>

<style>
</style>