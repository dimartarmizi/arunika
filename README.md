<img width="1920" height="1080" alt="arunika" src="https://github.com/user-attachments/assets/079f7438-1554-49dd-9fba-6a687ea5f2c8" />

# Arunika

[![Vue.js](https://img.shields.io/badge/Vue.js-4FC08D?style=for-the-badge&logo=vuedotjs&logoColor=white)](https://vuejs.org/) [![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)](https://vitejs.dev/) [![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)](https://tailwindcss.com/) [![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript) [![Node.js](https://img.shields.io/badge/Node.js-5FA04E?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org/)

A modern, lightweight, and responsive Admin Dashboard Template built with **Vue 3**, **Vite**, and **Tailwind CSS v4**.

Built using native Vue 3 components and semantic utility classes with zero bloat and no third-party UI framework dependencies.

---

## Key Features

- **Core Stack**: Vue 3 (Composition API, `<script setup>`) + Vite
- **Styling**: Tailwind CSS v4 with semantic design tokens via CSS variables
- **Theming**: Built-in Light and Dark Mode support
- **Routing**: File-based routing powered by `unplugin-vue-router`
- **Layout Variations**: Default vertical sidebar, horizontal nav, horizontal condensed, and boxed layouts
- **Ready-to-use Components**: Complete set of form inputs (inputs, custom selects, pickers, toggles) and UI elements (modals, tables, tabs, accordions, alerts, toasts, tooltips)
- **Icons**: Tabler Icons (`@tabler/icons-vue`)
- **Responsive Design**: Mobile-friendly drawers and responsive layout controls

---

## Directory Structure

```text
arunika/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   │   ├── form/          # Form input components
│   │   └── ui/            # UI elements
│   ├── composables/       # Shared logic (e.g., useTheme)
│   ├── layouts/           # App navigation shells (Navbar, Sidebar)
│   ├── pages/             # File-based routes
│   ├── App.vue
│   ├── main.js
│   └── style.css          # Design tokens & semantic component utilities
├── index.html
├── package.json
└── vite.config.js
```

---

## Getting Started

### Prerequisites

- **Node.js** >= 18.x
- Package manager (**npm**, **pnpm**, or **yarn**)

### Setup & Run

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

---

## License

[MIT License](LICENSE)
