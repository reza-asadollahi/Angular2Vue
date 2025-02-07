# Angular to Vue migration

## Quick comparison

1. **Project Structure**:
Angular: Highly opinionated with a specific folder structure and Angular CLI for scaffolding.

Vue.js: Less opinionated, more flexible structure. Vue CLI provides a basic structure but you're free to organize it your way.

2. **Templates**:
Angular: Uses TypeScript with HTML templates and Angular-specific syntax (*ngIf, *ngFor).

Vue.js: Uses HTML with directives (v-if, v-for). You can also use templates or Single File Components (.vue files).

3. **Components**:
Angular: Components are at the core. Defined using @Component decorator.

Vue.js: Components are also central. Defined using Vue.component or inside .vue files.

4. **Data Binding**:
Angular: Two-way binding using [(ngModel)].

Vue.js: Two-way binding using v-model.

5. **[Directives](#built-in-directives-in-vue)**:
Angular: Built-in directives like ngIf, ngFor, ngClass, ngStyle.

Vue.js: Similar built-in directives like v-if, v-for, v-bind, v-on.

6. **[Services and State Management](#state-managment-in-vue)**:
Angular: Uses services for dependency injection and state management with RxJS and NgRx.

Vue.js: Uses Vuex for state management. Services are not built-in but can be implemented using plugins.

7. **Routing**:
Angular: Uses @angular/router.

Vue.js: Uses vue-router.

8. **Lifecycle Hooks**:
Angular: Lifecycle hooks like ngOnInit, ngOnDestroy.

Vue.js: Similar lifecycle hooks like mounted, destroyed.

## Example Comparison:
### Angular Component:

``` typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',
  template: `<div *ngIf="show">Hello Angular</div>`,
  styleUrls: ['./example.component.css']
})
export class ExampleComponent {
  show = true;
}
```


#### Vue.jsComponent:

``` html
<template>
  <div v-if="show">Hello Vue.js</div>
</template>

<script>
export default {
  data() {
    return {
      show: true
    };
  }
};
</script>

<style scoped>
/* Add your styles here */
</style>
```

----------------------------

# Built in directives in Vue

1. **v-bind**:
Usage: Dynamically bind attributes or properties to expressions.

Example:

```html
<a v-bind:href="url">Link</a>
<!-- Shorthand: -->
<a :href="url">Link</a>
```

2. **v-model**:
Usage: Create two-way data bindings on form input, textarea, and select elements.

Example:

```html
<input v-model="message">
<p>{{ message }}</p>
```

3. **v-if, v-else-if, v-else**:
Usage: Conditionally render elements.

Example:

```html
<p v-if="seen">Now you see me</p>
<p v-else>Now you don't</p>
```

4. **v-for**:
Usage: Render a list of items by iterating over an array or object.

Example:

```html
<ul>
  <li v-for="item in items" :key="item.id">{{ item.text }}</li>
</ul>
```

5. **v-on**:
Usage: Attach event listeners that invoke methods.

Example:

```html
<button v-on:click="doSomething">Click me</button>
<!-- Shorthand: -->
<button @click="doSomething">Click me</button>
```

6. **v-show**:
Usage: Toggle the visibility of an element using CSS display property.

Example:

```html
<p v-show="isVisible">You can see me now!</p>
```

7. **v-bind:class & v-bind:style**:
Usage: Dynamically bind classes and styles.

Example:

```html
<div v-bind:class="{ active: isActive }">Class Binding</div>
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">Style Binding</div>
```

8. **v-pre**:
Usage: Skip compilation for this element and all its children.

Example:

```html
<span v-pre>{{ this will not be compiled }}</span>
```

9. **v-cloak**:
Usage: Keep Vue template content cloaked until the Vue instance finishes compiling.

Example:

```html
<div v-cloak>
  {{ message }}
</div>
```

10. **v-once**:
Usage: Render the element and component once, then skip updates.

Example:

```html
<span v-once>This will never change: {{ message }}</span>
Example Component Using Directives:
html
<template>
  <div>
    <input v-model="message" placeholder="Type something">
    <p v-if="message">{{ message }}</p>
    <button @click="toggleVisibility">Toggle Visibility</button>
    <p v-show="isVisible">This is conditionally visible</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: '',
      isVisible: true
    };
  },
  methods: {
    toggleVisibility() {
      this.isVisible = !this.isVisible;
    }
  }
};
</script>
```

These directives will give you powerful tools to manipulate the DOM and handle user interactions in Vue.js.If you have any specific questions or need further clarification, just let me know!



--------------------------------------------------------

# State Managment in Vue

The recommended state management library for Vue.jsis Vuex. Vuex helps you manage the state across your application in a centralized store, making it easier to manage and debug. Let's dive into the core concepts:

1. **State:**:
State in Vuex is similar to the data object in a Vue component. It holds the application data that needs to be shared across components.

Example:

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  }
});
```

2. **Getters:**:
Getters are used to access and compute derived state based on the store's state. They are similar to computed properties in Vue components.

Example:

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  getters: {
    doubleCount: state => state.count * 2
  }
});
```

3. **Mutations:**:
Mutations are the only way to directly change the state in Vuex. They are synchronous and must be committed.

Example:

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++;
    }
  }
});
```

4. **Actions:**:
Actions are similar to mutations, but they allow for asynchronous operations. Actions commit mutations to change the state.

Example:

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++;
    }
  },
  actions: {
    incrementAsync ({ commit }) {
      setTimeout(() => {
        commit('increment');
      }, 1000);
    }
  }
});
```

5. **Modules:**:
Modules allow you to split your store into smaller, manageable pieces with their own state, mutations, actions, and getters.

Example:

``` javascript
const moduleA = {
  state: () => ({ count: 0 }),
  mutations: {
    increment (state) {
      state.count++;
    }
  },
  actions: {
    incrementAsync ({ commit }) {
      setTimeout(() => {
        commit('increment');
      }, 1000);
    }
  },
  getters: {
    doubleCount: state => state.count * 2
  }
};

const store = new Vuex.Store({
  modules: {
    a: moduleA
  }
});
```

Putting It All Together:
Here's a simple example of a Vue component using Vuex state management:

``` html
<template>
  <div>
    <p>{{ count }}</p>
    <button @click="increment">Increment</button>
    <button @click="incrementAsync">Increment Async</button>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex';

export default {
  computed: {
    ...mapState([
      'count'
    ])
  },
  methods: {
    ...mapActions([
      'increment',
      'incrementAsync'
    ])
  }
};
</script>
```

## Summary:

- **State** holds the data.

- **Getters** access and compute derived state.

- **Mutations** are synchronous functions to change the state.

- **Actions** can be asynchronous and commit mutations.

- **Modules** help in organizing the store into smaller pieces.
