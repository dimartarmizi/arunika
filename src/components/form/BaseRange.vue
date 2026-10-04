<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
	modelValue: {
		type: [Number, String],
		default: 50
	},
	label: {
		type: String,
		default: ''
	},
	min: {
		type: [Number, String],
		default: 0
	},
	max: {
		type: [Number, String],
		default: 100
	},
	step: {
		type: [Number, String],
		default: 1
	},
	disabled: {
		type: Boolean,
		default: false
	}
})

defineEmits(['update:modelValue'])

const isHovered = ref(false)
const isDragging = ref(false)

const percentage = computed(() => {
	const minVal = Number(props.min)
	const maxVal = Number(props.max)
	const currentVal = Number(props.modelValue)
	if (maxVal <= minVal) return 0
	const pct = ((currentVal - minVal) / (maxVal - minVal)) * 100
	return Math.min(100, Math.max(0, pct))
})
</script>

<template>
	<div class="w-full">
		<label v-if="label" class="form-label flex items-center justify-between">
			<span>{{ label }}</span>
		</label>

		<div class="relative mt-2" @mouseenter="isHovered = true" @mouseleave="isHovered = false">
			<div :class="[
				'absolute -top-6 -translate-x-1/2 px-2 py-0.5 text-xs font-medium rounded-lg shadow-md bg-card text-foreground border border-border pointer-events-none transition-opacity duration-150',
				isHovered || isDragging ? 'opacity-100' : 'opacity-0'
			]" :style="{ left: `calc(${percentage}% + ${(0.5 - percentage / 100) * 16}px)` }">
				{{ modelValue }}
			</div>

			<input type="range" :min="min" :max="max" :step="step" :value="modelValue" :disabled="disabled" @input="$emit('update:modelValue', $event.target.value)" @mousedown="isDragging = true" @mouseup="isDragging = false" @touchstart="isDragging = true" @touchend="isDragging = false" class="w-full h-2 bg-muted rounded-lg appearance-none cursor-pointer accent-primary disabled:opacity-50 disabled:cursor-not-allowed" />
		</div>
	</div>
</template>
