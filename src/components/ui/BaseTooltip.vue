<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
	content: {
		type: String,
		required: true
	},
	position: {
		type: String,
		default: 'top',
		validator: (val) => ['top', 'bottom', 'left', 'right'].includes(val)
	},
	as: {
		type: String,
		default: 'span'
	}
})

const triggerRef = ref(null)
const isVisible = ref(false)
const tooltipStyle = ref({})

const show = () => {
	if (!triggerRef.value || !props.content) return
	const el = triggerRef.value.$el || triggerRef.value
	if (!el || typeof el.getBoundingClientRect !== 'function') return

	const rect = el.getBoundingClientRect()
	let top = 0
	let left = 0
	let transform = ''

	switch (props.position) {
		case 'top':
			top = rect.top - 6
			left = rect.left + rect.width / 2
			transform = 'translate(-50%, -100%)'
			break
		case 'bottom':
			top = rect.bottom + 6
			left = rect.left + rect.width / 2
			transform = 'translate(-50%, 0)'
			break
		case 'left':
			top = rect.top + rect.height / 2
			left = rect.left - 6
			transform = 'translate(-100%, -50%)'
			break
		case 'right':
			top = rect.top + rect.height / 2
			left = rect.right + 6
			transform = 'translate(0, -50%)'
			break
	}

	tooltipStyle.value = {
		top: `${top}px`,
		left: `${left}px`,
		transform
	}
	isVisible.value = true
}

const hide = () => {
	isVisible.value = false
}

onMounted(() => {
	window.addEventListener('scroll', hide, true)
})

onUnmounted(() => {
	window.removeEventListener('scroll', hide, true)
})
</script>

<template>
	<component :is="as" ref="triggerRef" class="inline-flex" @mouseenter="show" @mouseleave="hide" @focus="show" @blur="hide">
		<slot></slot>
		<Teleport to="body">
			<Transition enter-active-class="transition-opacity duration-150" enter-from-class="opacity-0" enter-to-class="opacity-100" leave-active-class="transition-opacity duration-100" leave-from-class="opacity-100" leave-to-class="opacity-0">
				<div v-if="isVisible && content" :style="tooltipStyle" class="fixed z-[9999] px-2.5 py-1 text-xs font-medium text-foreground bg-card border border-border rounded-lg shadow-lg whitespace-nowrap pointer-events-none">
					{{ content }}
				</div>
			</Transition>
		</Teleport>
	</component>
</template>
