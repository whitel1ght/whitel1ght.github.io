---
title: 'Dynamic Pinia Modules'
description: 'Creaing Pinia modules dynamically'
pubDate: 'Jul 08 2022'
heroImage: '/pine.jpg'
---

**TLDR;** This post explains how to dynamically create Pinia store modules in Vue projects, especially when dealing with multiple similar pages like admin tables. Instead of defining a separate store for each table, you can use a factory function to generate stores with customizable initial state. This approach reduces repetition and simplifies state management for features like paging across different tables. Example code demonstrates how to implement and use the factory for different tables.

It's easier to squeeze water out of a stone than to start writing a blog post. But here I am, writing my first and I hope you'll enjoy it. Or You might enjoy something completely different.


So, let's start with something fairly simple today. Imagine working on a project where you have a bunch of **similar** pages, and you need to create a Pinia state for each one. While the state may vary slightly, the approach I'm going to explain works well in scenarios where you need to repeat the same module creation multiple times.

**Where** might this happen, you ask? A typical case is an admin panel with several tables. What else?

- Managing filter states for different search forms across your app
- Handling settings or preferences for multiple user profiles
- Storing state for various dashboard widgets or cards
- Managing form data for multiple dynamic forms (e.g., surveys, wizards)
- Tracking state for different notification panels or message feeds

Let's focus on tables for our example. They may display independent entities, but you need to save table state that is common to all of them. Every table has paging, and your store modules will contain valuable data about each table.


The state for a table you might want to have can look like this:

```js
{
  page: 1,
  perPage: 10,
  descending: false
}
```

And here, instead of defining a Pinia module for each table page, you can create a Pinia factory that will take care of that.

```js
import { ref } from 'vue'
import { defineStore } from 'pinia'

function createPiniaStore ({ moduleName, state = {} }) {
  return defineStore(moduleName, () => {
    const initialState = {
      page: 1,
      perPage: 10,
      descending: false,
      ...state
    }
    const tableOptions = ref({ ...initialState })

    // you might want to reset state easily
    function reset() {
      Object.assign(tableOptions.value, initialState)
    }

    // you can put any actionsg/getters

    return { tableOptions }
  })
}

```

Later in your page component you can use it like that:

```js
import { createPiniaStore } from 'whatever'

const productsStore = createPiniaStore({
  moduleName: 'productsTable',
  // here you can set initial options for each table
  state: { perPage: 50 }
})

const usersStore = createPiniaStore({
  moduleName: 'usersTable',
  // here you can set initial options for each table
  state: { page: 2 }
})

```


#### Usage Caveats

- **Unique Module Names:** Each store must have a unique `moduleName`. If you reuse a name, Pinia will throw an error.
- **SSR Considerations:** If you're using server-side rendering, ensure that store state is isolated per request.
- **Reactivity:** The returned store is reactive, but if you add deeply nested objects, consider using `reactive` instead of `ref` for complex state.


Dynamic Pinia module creation is a powerful technique for managing similar stateful features across your Vue app, such as tables, forms, or widgets. By using a factory approach, you reduce boilerplate, keep your code DRY, and make your stores easier to maintain and extend. Whether you're building an admin panel or handling multiple dynamic components, this pattern can help you scale your state management with confidence. Happy coding!
