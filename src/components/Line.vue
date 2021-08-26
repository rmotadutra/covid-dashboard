<template>
	<path :d="d" :stroke="color" :stroke-width="2" fill="none"></path>
</template>

<script>
import { scaleLinear, scaleTime } from "d3-scale";
import { line } from "d3-shape";

export default {
	props: {
		x: {
			type: Number,
			required: true,
		},
		y: {
			type: Number,
			required: true,
		},
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
			type: String,
			required: true,
		},
		xlim: {
			type: Array,
			required: true,
		},
		ylim: {
			type: Array,
			required: true,
		},
		color: {
			type: String,
			required: true,
		},
	},
	computed: {
		xScale() {
			return scaleTime()
				.rangeRound([this.x, this.x + this.width])
				.domain([new Date(this.xlim[0]), new Date(this.xlim[1])]);
		},
		yScale() {
			return scaleLinear()
				.range([this.height, this.y])
				.domain([this.ylim[0], this.ylim[1]]);
		},
		lineGenerator() {
			return line()
				.x((d) => this.xScale(new Date(d.date)))
				.y((d) => this.yScale(d[this.type]));
		},
		d() {
			return this.lineGenerator(this.data);
		},
	},
};
</script>

<style>
</style>