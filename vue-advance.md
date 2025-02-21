Since you're already working with Vue (and Nuxt), let's go beyond the basics and cover some advanced Vue.js concepts that will help you build more scalable and maintainable applications.

1. Vue 3 Composition API - Advanced Usage

Vue 3 introduced the Composition API, which makes complex logic more reusable and manageable.

Example: Using provide and inject
Instead of prop-drilling, use provide and inject to share data across deeply nested components.

``` html
<!-- Parent.vue -->
<script setup>
import { provide, ref } from 'vue';

const theme = ref('dark');
provide('theme', theme);
</script>

<template>
  <Child />
</template>

<!-- Child.vue -->
<script setup>
import { inject } from 'vue';

const theme = inject('theme');
</script>

<template>
  <p>Current Theme: {{ theme }}</p>
</template>
```
🔹 Use Case: This is useful for global state management without Vuex or Pinia.

2. Using watchEffect for Reactive Dependencies

watchEffect automatically tracks dependencies and runs when they change.

``` html
<script setup>
import { ref, watchEffect } from 'vue';

const count = ref(0);

watchEffect(() => {
  console.log(`Count is now: ${count.value}`);
});
</script>

<template>
  <button @click="count++">Increment</button>
</template>
```

🔹 Use Case: Useful for debugging, API calls, or reactivity tracking.

3. Vue Custom Directives (v-directive)

You can create custom directives for reusable DOM manipulation logic.

Example: Auto-focus Directive
``` html
<!-- v-focus directive -->
<script>
export default {
  directives: {
    focus: {
      mounted(el) {
        el.focus();
      }
    }
  }
}
</script>

<template>
  <input v-focus />
</template>
```
🔹 Use Case: Use this to automatically focus on input fields when the component mounts.

4. Dynamic Components with <component>

Dynamically render different components based on some conditions.

``` html
<template>
  <component :is="currentComponent" />
</template>

<script setup>
import { ref } from 'vue';
import ComponentA from './ComponentA.vue';
import ComponentB from './ComponentB.vue';

const currentComponent = ref('ComponentA');

// Switch component dynamically
setTimeout(() => {
  currentComponent.value = 'ComponentB';
}, 3000);
</script>
```

🔹 Use Case: Useful for tab systems, modals, or dashboards.

5. Suspense and Async Components

Vue 3 has <Suspense> to handle async components.

``` html
<template>
  <Suspense>
    <template #default>
      <AsyncComponent />
    </template>
    <template #fallback>
      <p>Loading...</p>
    </template>
  </Suspense>
</template>

<script setup>
import { defineAsyncComponent } from 'vue';

const AsyncComponent = defineAsyncComponent(() =>
  import('./ExpensiveComponent.vue')
);
</script>
```

🔹 Use Case: Great for lazy loading components like modals, dashboards, or large lists.

6. Vue Teleport (Rendering Outside the DOM Tree)

Sometimes, you need to render components outside their parent, like modals.

``` html
<template>
  <Teleport to="body">
    <div class="modal">
      <p>This is a modal rendered outside the parent component!</p>
    </div>
  </Teleport>
</template>
```

🔹 Use Case: Best for modals, notifications, or tooltips.

7. Pinia - Modern State Management

Vuex is deprecated, and Pinia is now the recommended state management library.

Example: Global Store
``` js
// store.js
import { defineStore } from 'pinia';

export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0
  }),
  actions: {
    increment() {
      this.count++;
    }
  }
});
```
Using the Store in a Component

``` html
<script setup>
import { useCounterStore } from '@/store';

const store = useCounterStore();
</script>

<template>
  <p>Count: {{ store.count }}</p>
  <button @click="store.increment">Increment</button>
</template>
```
🔹 Use Case: Pinia is reactive, modular, and lightweight—better than Vuex.

8. Vue Router - Advanced Navigation Guards

Vue Router allows navigation guards for authentication.

Example: Protecting Routes
``` js
// router.js
import { createRouter, createWebHistory } from 'vue-router';
import Home from '@/views/Home.vue';
import Dashboard from '@/views/Dashboard.vue';

const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: '/', component: Home },
    { path: '/dashboard', component: Dashboard, meta: { requiresAuth: true } }
  ]
});

// Global Navigation Guard
router.beforeEach((to, from, next) => {
  const isAuthenticated = localStorage.getItem('user');
  if (to.meta.requiresAuth && !isAuthenticated) {
    next('/');
  } else {
    next();
  }
});

export default router;
```

🔹 Use Case: Protect pages like admin dashboards or user profiles.

9. Vue Use (@vueuse/core) - Advanced Utilities

@vueuse/core is a powerful collection of Vue utilities.

Example: Using useDark for Dark Mode
``` html
<script setup>
import { useDark, useToggle } from '@vueuse/core';

const isDark = useDark();
const toggleDark = useToggle(isDark);
</script>

<template>
  <button @click="toggleDark()">Toggle Dark Mode</button>
</template>
```
🔹 Use Case: Easily manage dark mode, clipboard, sensors, and more.

10. Server-Side Rendering (SSR) with Nuxt 3

Since you're using Nuxt, take advantage of server-side rendering (SSR).

Example: Fetch Data on the Server
``` html
<script setup>
const { data } = await useFetch('/api/posts');
</script>

<template>
  <ul>
    <li v-for="post in data" :key="post.id">{{ post.title }}</li>
  </ul>
</template>
```
🔹 Use Case: SSR is great for SEO, performance, and pre-rendering data.
