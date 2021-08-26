<template>
	<g
		ref="xGrid"
		stroke="#e0e0e0"
		:stroke-width="0.15"
		:transform="`translate(${0}, ${0})`"
	></g>
	<g
		ref="yGrid"
		stroke="#e0e0e0"
		:stroke-width="0.15"
		:transform="`translate(${x}, ${0})`"
	></g>
</template>

<script>
import { scaleLinear, scaleTime } from "d3-scale";
import { axisLeft, axisBottom } from "d3-axis";
import { select } from "d3-selection";

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
		xlim: {
			type: Array,
			required: true,
		},
		ylim: {
			type: Array,
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
				.range([this.y + this.height, this.y])
				.domain([this.ylim[0], this.ylim[1]]);
		},
		xGrid() {
			return axisBottom(this.xScale)
				.ticks(20)
				.tickSize(this.height)
				.tickFormat("");
		},
		yGrid() {
			return axisLeft(this.yScale)
				.ticks(10)
				.tickSize(-this.width)
				.tickFormat("");
		},
	},
	mounted() {
		select(this.$refs.xGrid).call(this.xGrid);
		select(this.$refs.yGrid).call(this.yGrid);
	},
};
</script>

<style>
</style>