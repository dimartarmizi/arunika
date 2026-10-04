<script setup>
import { IconUpload, IconFile, IconX } from '@tabler/icons-vue'
import { ref, computed } from 'vue'

const props = defineProps({
	label: {
		type: String,
		default: ''
	},
	placeholder: {
		type: String,
		default: 'No file chosen'
	},
	dropzone: {
		type: Boolean,
		default: false
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

const emit = defineEmits(['change'])
const fileInputRef = ref(null)
const fileName = ref('')

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

const handleFileChange = (e) => {
	const file = e.target.files?.[0]
	fileName.value = file ? file.name : ''
	emit('change', e)
}

const clearFile = (e) => {
	e.stopPropagation()
	if (fileInputRef.value) {
		fileInputRef.value.value = ''
	}
	fileName.value = ''
	emit('change', { target: { files: [] } })
}
</script>

<template>
	<div class="w-full">
		<label v-if="label" class="form-label flex items-center justify-between">
			<span>
				{{ label }}
				<span v-if="required" class="text-destructive font-bold ml-0.5">*</span>
			</span>
		</label>

		<label v-if="dropzone" :class="[
			'relative flex items-center justify-center gap-2 px-4 h-[42px] border-2 border-dashed rounded-xl transition group select-none',
			disabled ? 'opacity-50 cursor-not-allowed bg-muted border-border' : 'cursor-pointer hover:border-primary bg-card/60 hover:bg-primary-soft/40',
			computedState === 'error' ? 'border-destructive bg-destructive-soft' : '',
			computedState === 'success' ? 'border-success bg-success-soft' : '',
			computedState === 'warning' ? 'border-warning bg-warning-soft' : '',
			!computedState && !disabled ? 'border-border' : ''
		]">
			<IconUpload :size="18" class="text-muted-foreground group-hover:text-primary transition shrink-0" />
			<span class="text-xs truncate max-w-[calc(100%-80px)] text-muted-foreground">
				<template v-if="fileName">
					<span class="font-medium text-foreground">{{ fileName }}</span>
				</template>
				<template v-else>
					<span class="font-semibold text-primary">Choose a file</span> or drag it here
				</template>
			</span>
			<button v-if="fileName && !disabled" type="button" @click="clearFile" class="text-muted-foreground hover:text-destructive p-0.5 rounded cursor-pointer transition ml-1" title="Remove file">
				<IconX :size="14" />
			</button>
			<input ref="fileInputRef" type="file" :disabled="disabled" class="hidden" @change="handleFileChange" />
		</label>

		<label v-else :class="[
			'relative flex items-center h-[42px] px-1 border rounded-xl bg-card transition select-none group',
			disabled ? 'opacity-50 cursor-not-allowed' : 'cursor-pointer hover:border-primary',
			computedState === 'error' ? 'border-destructive' : '',
			computedState === 'success' ? 'border-success' : '',
			computedState === 'warning' ? 'border-warning' : '',
			!computedState ? 'border-border' : ''
		]">
			<span class="btn btn-sm btn-primary shrink-0 pointer-events-none rounded-lg px-3 py-1 font-medium text-xs">
				Choose File
			</span>

			<div class="flex items-center gap-1.5 ml-3 flex-1 min-w-0 pr-2">
				<IconFile v-if="fileName" :size="14" class="text-muted-foreground shrink-0" />
				<span :class="['text-xs truncate', fileName ? 'text-foreground font-medium' : 'text-muted-foreground']">
					{{ fileName || placeholder }}
				</span>
			</div>

			<button v-if="fileName && !disabled" type="button" @click="clearFile" class="text-muted-foreground hover:text-destructive p-1 rounded cursor-pointer transition shrink-0" title="Remove file">
				<IconX :size="14" />
			</button>

			<input ref="fileInputRef" type="file" :disabled="disabled" class="hidden" @change="handleFileChange" />
		</label>

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
