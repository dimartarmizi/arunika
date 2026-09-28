<script setup>
import { Link } from '@inertiajs/vue3'
import { IconChevronRight } from '@tabler/icons-vue'

defineProps({
	items: {
		type: Array,
		default: () => [],
	},
})
</script>

<template>
	<nav class="flex items-center gap-1.5 text-xs text-muted-foreground">
		<template v-for="(item, index) in items" :key="index">
			<IconChevronRight v-if="index > 0" :size="14" class="text-muted-foreground/60 shrink-0" />
			<Link v-if="item.href && index < items.length - 1" :href="item.href" class="hover:text-foreground transition truncate">
				{{ item.label }}
			</Link>
			<span v-else :class="[
				index === items.length - 1 ? 'font-medium text-foreground' : '',
				'truncate'
			]">
				{{ item.label }}
			</span>
		</template>
	</nav>
</template>
