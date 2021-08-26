<template>
	<label :for="id" class="label">
		<input
			:id="id"
			class="checkbox"
			type="checkbox"
			:checked="modelValue"
			@input="$emit('update:modelValue', $event.target.checked)"
		/>
		{{ label }}
	</label>
</template>

<script>
export default {
	props: {
		id: {
			type: String,
		},
		label: {
			type: String,
		},
		modelValue: {
			type: Boolean,
		},
	},
};
</script>

<style>
	.label {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		font-size: 1.2rem;
		margin-right: 10px;
		color: #1e1e1e;
		cursor: pointer;
		display: flex;
		align-content: center;
		align-items: center;
	}

	.checkbox {
		zoom: 2;
		margin: 2px;
		cursor: pointer;
	}
</style>