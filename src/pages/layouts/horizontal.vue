<script setup>
import BaseTable from '../../components/ui/BaseTable.vue'
import { useTheme } from '../../composables/useTheme'
import {
	IconBuildingStore,
	IconBadge,
	IconLayoutDashboard,
	IconForms,
	IconTable,
	IconClick,
	IconFolders,
	IconLayoutList,
	IconComponents,
	IconAppWindow,
	IconBell,
	IconNotification,
	IconHelpCircle,
	IconChevronDown,
	IconSearch,
	IconPencil,
	IconTrash,
	IconUsers,
	IconEye,
	IconUser,
	IconLock,
	IconLogout,
	IconMenu2,
	IconX,
	IconSun,
	IconMoon
} from '@tabler/icons-vue'
import { ref, onMounted, onUnmounted, watch } from 'vue'
import { useRoute } from 'vue-router'

const { isDark, toggleTheme } = useTheme()
const route = useRoute()
const activeDropdown = ref(null)
const profileOpen = ref(false)
const profileDropdownRef = ref(null)
const mobileMenuOpen = ref(false)
const mobileSubmenu = ref(null)

const toggleDropdown = (key) => {
	activeDropdown.value = activeDropdown.value === key ? null : key
}

const toggleMobileSubmenu = (key) => {
	mobileSubmenu.value = mobileSubmenu.value === key ? null : key
}

const toggleProfile = () => {
	profileOpen.value = !profileOpen.value
}

const navItems = [
	{ to: '/', label: 'Dashboard', icon: IconLayoutDashboard },
	{
		key: 'forms',
		label: 'Forms & Tables',
		icon: IconForms,
		children: [
			{ to: '/form-elements', label: 'Form Elements', icon: IconForms },
			{ to: '/tables', label: 'Data Tables', icon: IconTable }
		]
	},
	{
		key: 'ui',
		label: 'Components',
		icon: IconComponents,
		children: [
			{ to: '/accordion', label: 'Accordion', icon: IconLayoutList },
			{ to: '/alerts', label: 'Alerts', icon: IconBell },
			{ to: '/badges', label: 'Badges', icon: IconBadge },
			{ to: '/buttons', label: 'Buttons', icon: IconClick },
			{ to: '/modals', label: 'Modals', icon: IconAppWindow },
			{ to: '/tabs', label: 'Tabs', icon: IconFolders },
			{ to: '/toasts', label: 'Toasts', icon: IconNotification },
			{ to: '/tooltips', label: 'Tooltips', icon: IconHelpCircle }
		]
	},
	{ to: '/users', label: 'Users', icon: IconUsers }
]

const closeDropdowns = (e) => {
	activeDropdown.value = null
	if (profileDropdownRef.value && !profileDropdownRef.value.contains(e?.target)) {
		profileOpen.value = false
	}
}

watch(() => route.path, () => {
	mobileMenuOpen.value = false
})

onMounted(() => {
	window.addEventListener('click', closeDropdowns)
})

onUnmounted(() => {
	window.removeEventListener('click', closeDropdowns)
})

const metrics = [
	{ label: 'Total Revenue', value: '$48,250', change: '+12.5%', isUp: true },
	{ label: 'Active Users', value: '2,840', change: '+8.1%', isUp: true },
	{ label: 'Pending Orders', value: '43', change: '-3.2%', isUp: false },
	{ label: 'Conversion Rate', value: '3.62%', change: '+0.8%', isUp: true }
]

const columns = [
	{ key: 'product', label: 'Product Name' },
	{ key: 'category', label: 'Category' },
	{ key: 'stock', label: 'Stock', align: 'center' },
	{ key: 'price', label: 'Price', align: 'right' },
	{ key: 'status', label: 'Status', align: 'center' },
	{ key: 'actions', label: 'Actions', align: 'right' }
]

const products = ref([
	{ id: 1, product: 'Wireless Noise-Canceling Headphones', category: 'Audio', stock: 45, price: '$299.00', status: 'In Stock' },
	{ id: 2, product: 'Ergonomic Mechanical Keyboard', category: 'Peripherals', stock: 12, price: '$149.00', status: 'Low Stock' },
	{ id: 3, product: 'Ultra-Wide Curved Gaming Monitor', category: 'Displays', stock: 0, price: '$699.00', status: 'Out of Stock' },
	{ id: 4, product: 'Thunderbolt 4 Docking Station', category: 'Accessories', stock: 88, price: '$199.00', status: 'In Stock' },
	{ id: 5, product: 'Smart RGB Desk Lamp Pro', category: 'Lighting', stock: 24, price: '$79.00', status: 'In Stock' }
])
</script>

<template>
	<div class="min-h-screen bg-background flex flex-col text-foreground antialiased font-sans" @click="closeDropdowns">
		<div v-if="mobileMenuOpen" @click="mobileMenuOpen = false" class="fixed inset-0 z-50 bg-black/50 backdrop-blur-xs md:hidden transition-opacity"></div>

		<aside :class="[
			'fixed inset-y-0 left-0 z-50 w-72 bg-card text-foreground border-r border-border shadow-2xl flex flex-col transition-transform duration-300 ease-in-out md:hidden',
			mobileMenuOpen ? 'translate-x-0' : '-translate-x-full'
		]" @click.stop>
			<div class="h-16 flex items-center justify-between px-4 border-b border-border">
				<router-link to="/" class="flex items-center gap-2" @click="mobileMenuOpen = false">
					<div class="icon-box icon-box-md icon-box-primary shadow-sm shrink-0">
						<IconBuildingStore :size="20" />
					</div>
					<span class="text-base font-bold text-foreground tracking-tight">Arunika</span>
				</router-link>
				<button @click="mobileMenuOpen = false" class="p-1.5 rounded-lg text-muted-foreground hover:text-foreground hover:bg-muted cursor-pointer" aria-label="Close menu">
					<IconX :size="20" />
				</button>
			</div>

			<div class="px-4 py-2.5 border-b border-border flex items-center gap-2">
				<button class="flex-1 flex items-center justify-center gap-2 p-2 rounded-xl text-xs font-medium text-muted-foreground hover:text-foreground bg-muted/50 hover:bg-muted transition cursor-pointer" title="Search">
					<IconSearch :size="16" />
					<span>Search</span>
				</button>
				<button @click="toggleTheme" class="p-2 rounded-xl text-muted-foreground hover:text-foreground bg-muted/50 hover:bg-muted transition cursor-pointer" :title="isDark ? 'Switch to Light' : 'Switch to Dark'">
					<IconSun v-if="isDark" :size="16" />
					<IconMoon v-else :size="16" />
				</button>
				<button class="p-2 rounded-xl text-muted-foreground hover:text-foreground bg-muted/50 hover:bg-muted transition relative cursor-pointer" title="Notifications">
					<IconBell :size="16" />
					<span class="absolute top-1.5 right-1.5 w-2 h-2 rounded-full bg-primary ring-2 ring-card"></span>
				</button>
			</div>

			<div class="p-3 flex-1 overflow-y-auto flex flex-col gap-1">
				<template v-for="item in navItems" :key="item.label">
					<router-link v-if="!item.children" :to="item.to" @click="mobileMenuOpen = false" class="flex items-center gap-2.5 px-3 py-2 rounded-lg text-sm font-medium transition hover:bg-muted" :class="route.path === item.to ? 'bg-primary-soft text-primary' : 'text-foreground'">
						<component :is="item.icon" :size="18" />
						<span>{{ item.label }}</span>
					</router-link>

					<div v-else>
						<button type="button" @click="toggleMobileSubmenu(item.key)" class="w-full flex items-center justify-between px-3 py-2 rounded-lg text-sm font-medium text-foreground hover:bg-muted transition cursor-pointer">
							<span class="flex items-center gap-2.5">
								<component :is="item.icon" :size="18" class="text-foreground" />
								<span>{{ item.label }}</span>
							</span>
							<IconChevronDown :size="16" :class="['transition-transform duration-200', mobileSubmenu === item.key ? 'rotate-180' : '']" />
						</button>
						<div v-if="mobileSubmenu === item.key" class="relative ml-5 pl-4 py-1 flex flex-col gap-0.5">
							<router-link v-for="(sub, subIdx) in item.children" :key="sub.to" :to="sub.to" @click="mobileMenuOpen = false" class="relative flex items-center gap-2 px-3 py-1.5 rounded-lg text-sm font-medium text-foreground hover:bg-muted transition" :class="route.path === sub.to ? 'bg-primary-soft text-primary!' : ''">
								<span class="absolute -left-4 w-px bg-border pointer-events-none" :class="subIdx === item.children.length - 1 ? 'top-0 h-1/2' : 'top-0 h-full'"></span>
								<span class="absolute -left-4 top-1/2 w-3 h-px bg-border pointer-events-none"></span>

								<component :is="sub.icon" :size="16" />
								<span>{{ sub.label }}</span>
							</router-link>
						</div>
					</div>
				</template>
			</div>
		</aside>

		<header class="sticky top-0 z-40 bg-card border-b border-border/80 shadow-xs">
			<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between gap-4">
				<div class="flex items-center gap-2 shrink-0">
					<button type="button" @click.stop="mobileMenuOpen = !mobileMenuOpen" class="md:hidden p-2 rounded-xl text-muted-foreground hover:text-foreground hover:bg-muted transition cursor-pointer" aria-label="Toggle menu">
						<IconMenu2 :size="20" />
					</button>

					<router-link to="/" class="flex items-center gap-2 shrink-0">
						<div class="icon-box icon-box-md icon-box-primary shadow-sm">
							<IconBuildingStore :size="20" />
						</div>
						<span class="text-base font-bold text-foreground tracking-tight leading-none">Arunika</span>
					</router-link>
				</div>

				<div class="flex items-center gap-2">
					<button class="hidden md:flex p-2 rounded-xl text-muted-foreground hover:text-foreground hover:bg-muted transition cursor-pointer" title="Search">
						<IconSearch :size="18" />
					</button>

					<button @click="toggleTheme" class="hidden md:flex p-2 rounded-xl text-muted-foreground hover:text-foreground hover:bg-muted transition cursor-pointer" :title="isDark ? 'Switch to Light' : 'Switch to Dark'">
						<IconSun v-if="isDark" :size="18" />
						<IconMoon v-else :size="18" />
					</button>

					<button class="hidden md:flex p-2 rounded-xl text-muted-foreground hover:text-foreground hover:bg-muted transition relative cursor-pointer" title="Notifications">
						<IconBell :size="18" />
						<span class="absolute top-1.5 right-1.5 w-2 h-2 rounded-full bg-primary ring-2 ring-card"></span>
					</button>

					<div class="h-6 w-px bg-muted hidden md:block mx-1"></div>

					<div class="relative" ref="profileDropdownRef" @click.stop>
						<button type="button" @click="toggleProfile" class="flex items-center gap-2 p-1.5 rounded-xl hover:bg-muted transition cursor-pointer select-none text-left" aria-label="User menu">
							<img class="avatar avatar-sm ring-2 ring-border shrink-0" src="https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?w=120" alt="Avatar" />
							<div class="hidden sm:flex flex-col text-left">
								<span class="text-xs font-semibold text-foreground">Administrator</span>
								<span class="text-[10px] text-muted-foreground">admin@arunika.io</span>
							</div>
						</button>

						<div v-if="profileOpen" class="absolute right-0 mt-2 w-56 bg-card border border-border rounded-2xl shadow-xl py-2 z-50 animate-in fade-in zoom-in-95 duration-100">
							<div class="px-4 py-2.5 border-b border-border">
								<p class="text-xs font-bold text-foreground truncate">Administrator</p>
								<p class="text-[11px] text-muted-foreground truncate">admin@arunika.io</p>
							</div>

							<div class="py-1">
								<router-link to="/users" @click="profileOpen = false" class="flex items-center gap-2.5 px-4 py-2 text-xs font-medium text-muted-foreground hover:bg-muted hover:text-foreground transition">
									<IconUser :size="16" class="text-muted-foreground" />
									<span>Account Profile</span>
								</router-link>
								<router-link to="/reset-password" @click="profileOpen = false" class="flex items-center gap-2.5 px-4 py-2 text-xs font-medium text-muted-foreground hover:bg-muted hover:text-foreground transition">
									<IconLock :size="16" class="text-muted-foreground" />
									<span>Change Password</span>
								</router-link>
							</div>

							<div class="pt-1 border-t border-border">
								<router-link to="/login" @click="profileOpen = false" class="flex items-center gap-2.5 px-4 py-2 text-xs font-semibold text-destructive hover:bg-destructive-soft transition">
									<IconLogout :size="16" />
									<span>Log Out</span>
								</router-link>
							</div>
						</div>
					</div>
				</div>
			</div>

			<nav class="border-t border-border bg-card hidden md:block">
				<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex items-center gap-1 overflow-visible">
					<template v-for="item in navItems" :key="item.label">
						<router-link v-if="!item.children" :to="item.to" :class="[
							'flex items-center gap-2 px-3.5 py-3 text-sm font-medium border-b-2 transition',
							route.path === item.to
								? 'border-primary text-primary'
								: 'border-transparent text-muted-foreground hover:text-foreground hover:border-border'
						]">
							<component :is="item.icon" :size="18" />
							<span>{{ item.label }}</span>
						</router-link>

						<div v-else class="relative" @click.stop>
							<button @click="toggleDropdown(item.key)" :class="[
								'flex items-center gap-1.5 px-3.5 py-3 text-sm font-medium border-b-2 transition cursor-pointer',
								activeDropdown === item.key
									? 'border-primary text-primary bg-background/50'
									: 'border-transparent text-muted-foreground hover:text-foreground hover:border-border'
							]">
								<component :is="item.icon" :size="18" />
								<span>{{ item.label }}</span>
								<IconChevronDown :size="16" :class="['transition-transform duration-200', activeDropdown === item.key ? 'rotate-180' : '']" />
							</button>

							<div v-if="activeDropdown === item.key" class="absolute top-full left-0 mt-1 min-w-[13rem] bg-card rounded-xl shadow-xl border border-border/80 p-1.5 z-50 flex flex-col gap-0.5">
								<router-link v-for="sub in item.children" :key="sub.to" :to="sub.to" @click="closeDropdowns" class="flex items-center gap-2.5 px-3 py-2 rounded-lg text-sm text-muted-foreground hover:bg-muted hover:text-foreground transition" :class="route.path === sub.to ? 'bg-primary-soft text-primary' : ''">
									<component :is="sub.icon" :size="18" class="text-muted-foreground" />
									<span>{{ sub.label }}</span>
								</router-link>
							</div>
						</div>
					</template>
				</div>
			</nav>
		</header>

		<main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 flex flex-col gap-4">
			<section aria-label="Key metrics" class="flex flex-col sm:flex-row flex-wrap gap-4">
				<article v-for="metric in metrics" :key="metric.label" class="card p-5 flex-1 min-w-[200px] flex flex-col justify-between">
					<span class="text-xs font-medium text-muted-foreground">{{ metric.label }}</span>
					<div class="flex items-baseline justify-between mt-2">
						<span class="text-2xl font-bold text-foreground">{{ metric.value }}</span>
						<span :class="['text-xs font-semibold', metric.isUp ? 'text-success' : 'text-destructive']">
							{{ metric.change }}
						</span>
					</div>
				</article>
			</section>

			<div class="card overflow-hidden">
				<div class="card-header flex items-center justify-between">
					<div>
						<h4 class="font-semibold text-sm text-foreground">Products Catalog</h4>
						<p class="text-xs text-muted-foreground">Standard table with cell slots for custom badges & action buttons</p>
					</div>
				</div>
				<BaseTable :columns="columns" :data="products">
					<template #cell(product)="{ value }">
						<span class="font-medium text-foreground text-xs sm:text-sm">{{ value }}</span>
					</template>

					<template #cell(category)="{ value }">
						<span class="text-xs text-muted-foreground">{{ value }}</span>
					</template>

					<template #cell(stock)="{ value }">
						<span class="text-xs font-semibold text-muted-foreground">{{ value }}</span>
					</template>

					<template #cell(price)="{ value }">
						<span class="text-xs sm:text-sm font-semibold text-foreground">{{ value }}</span>
					</template>

					<template #cell(status)="{ value }">
						<span :class="[
							'badge',
							value === 'In Stock' ? 'badge-success' :
								value === 'Low Stock' ? 'badge-warning' : 'badge-destructive'
						]">
							{{ value }}
						</span>
					</template>

					<template #cell(actions)>
						<div class="inline-flex items-center gap-1.5">
							<button class="btn btn-ghost btn-icon" title="View details">
								<IconEye :size="16" />
							</button>
							<button class="btn btn-ghost btn-icon hover:text-primary" title="Edit item">
								<IconPencil :size="16" />
							</button>
							<button class="btn btn-ghost btn-icon hover:text-destructive" title="Delete item">
								<IconTrash :size="16" />
							</button>
						</div>
					</template>
				</BaseTable>
			</div>
		</main>
	</div>
</template>
