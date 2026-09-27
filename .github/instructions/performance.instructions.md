---
description: 'Performance rules for lazy loading, bundle size, re-renders, and change detection. Use when adding UI, dependencies, or reactive updates.'
applyTo: '**'
---

# Performance

- Lazy-load routes and features that are not required for first paint.
- Check the bundle cost before adding a dependency. Prefer a platform API or a dependency the app already ships.
- Import the symbol you use. Avoid pulling in an entire library for one helper.
- In React, keep Server Components on the server. Isolate `"use client"` so a parent re-render does not redraw a static tree.
- Avoid passing fresh object, array, or function identities into memoized children on every render.
- In Angular, keep `OnPush`. Derive template values with `computed` instead of calling allocating methods from the template.
- Update the smallest slice of state that changed. Do not refresh a whole store to change one value.
