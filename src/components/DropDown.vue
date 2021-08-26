<template>
	<select :id="id || name" :name="name || id" @change="onChange">
		<option value="">{{ label }}</option>
		<option v-for="{ label, value } in options" :key="value" :value="value">
			{{ label }}
		</option>
	</select>
</template>

<script>
export default {
	props: {
		id: {
			type: String,
		},
		name: {
			type: String,
			required: true,
		},
		label: {
			type: String,
		},
		options: {
			required: true,
			default: [],
		},
		onChange: {
			type: Function,
		},
	},

	data() {
		return {
			selected: null,
		};
	},
};
</script>

<style scoped>
	select {
		width: 100%;
		max-width: 250px;
		padding: 5px 0;
		margin: 5px 5px 5px;
		font-size: 1.2rem;
		border: solid 1px rgba(0, 0, 0, 0.3);
		border-radius: 0.3rem;
		background-color: #e1e1e1;
		color: #1e1e1e;
	}

	select:focus-visible {
		outline-color: rgba(0, 0, 0, 0.6);
	}
</style>