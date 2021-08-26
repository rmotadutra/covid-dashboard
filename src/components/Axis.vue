<template>
	<g ref="xAxis" :transform="`translate(${0}, ${height - axisOffset})`"></g>
	<g ref="yAxis" :transform="`translate(${axisOffset}, ${0})`"></g>
	<text :x="width / 2" :y="height - axisOffset / 4">Date</text>
	<text
		:x="-(height - axisOffset) / 2"
		:y="axisOffset / 4"
		transform="rotate(-90)"
		:text-anchor="'middle'"
	>
		{{ ylabel }}
	</text>
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
		ylabel: {
			type: String,
			required: true,
		},
	},
	data() {
		return {
			axisOffset: 100,
		};
	},
	computed: {
		xScale() {
			return scaleTime()
				.rangeRound([this.x, this.x + this.width])
				.domain([new Date(this.xlim[0]), new Date(this.xlim[1])]);
		},
		yScale() {
			return scaleLinear()
				.range([this.y + (this.height - this.axisOffset), this.y])
				.domain([this.ylim[0], this.ylim[1]]);
		},
		xAxis() {
			return axisBottom(this.xScale).ticks(20);
		},
		yAxis() {
			return axisLeft(this.yScale).ticks(10);
		},
	},
	mounted() {
		select(this.$refs.xAxis)
			.call(this.xAxis)
			.selectAll("text")
			.attr("transform", "translate(-8,1)rotate(-45)")
			.style("text-anchor", "end");
		select(this.$refs.yAxis).call(this.yAxis);
	},
};
</script>

<style>
</style>