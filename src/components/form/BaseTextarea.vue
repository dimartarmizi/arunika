<script setup>
import { IconAlertCircle, IconCircleCheck, IconAlertTriangle } from '@tabler/icons-vue'
import { computed } from 'vue'

const props = defineProps({
	modelValue: {
		type: [String, Number],
		default: ''
	},
	label: {
		type: String,
		default: ''
	},
	rows: {
		type: [String, Number],
		default: 3
	},
	placeholder: {
		type: String,
		default: ''
	},
	state: {
		type: String,
		default: null,
		validator: (val) => [null, 'error', 'success', 'warning'].includes(val)
	},
	error: {
		type: [String, Boolean],
		default: false
	},
	success: {
		type: [String, Boolean],
		default: false
	},
	warning: {
		type: [String, Boolean],
		default: false
	},
	hint: {
		type: String,
		default: ''
	},
	required: {
		type: Boolean,
		default: false
	},
	disabled: {
		type: Boolean,
		default: false
	}
})

defineEmits(['update:modelValue'])

const computedState = computed(() => {
	if (props.error) return 'error'
	if (props.success) return 'success'
	if (props.warning) return 'warning'
	return props.state || null
})

const feedbackMessage = computed(() => {
	if (typeof props.error === 'string' && props.error) return props.error
	if (typeof props.success === 'string' && props.success) return props.success
	if (typeof props.warning === 'string' && props.warning) return props.warning
	return props.hint || ''
})
</script>

<template>
	<div class="w-full min-w-0">
		<label v-if="label" class="form-label flex items-center justify-between">
			<span>
				{{ label }}
				<span v-if="required" class="text-destructive font-bold ml-0.5">*</span>
			</span>
			<slot name="label-extra" />
		</label>

		<div class="relative">
			<textarea :value="modelValue" :placeholder="placeholder" :rows="rows" :disabled="disabled" :required="required" @input="$emit('update:modelValue', $event.target.value)" v-bind="$attrs" :class="[
				'textarea resize-y',
				computedState === 'error' ? 'input-error' : '',
				computedState === 'success' ? 'input-success' : '',
				computedState === 'warning' ? 'input-warning' : ''
			]"></textarea>

			<div v-if="computedState" class="absolute top-3 right-3 pointer-events-none">
				<IconAlertCircle v-if="computedState === 'error'" :size="18" class="text-destructive" />
				<IconCircleCheck v-else-if="computedState === 'success'" :size="18" class="text-success" />
				<IconAlertTriangle v-else-if="computedState === 'warning'" :size="18" class="text-warning" />
			</div>
		</div>

		<p v-if="feedbackMessage" :class="[
			computedState === 'error' ? 'form-hint-error' : '',
			computedState === 'success' ? 'form-hint-success' : '',
			computedState === 'warning' ? 'form-hint-warning' : '',
			!computedState ? 'form-hint' : ''
		]">
			<span>{{ feedbackMessage }}</span>
		</p>
	</div>
</template>
