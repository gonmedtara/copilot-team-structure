---
description: 'React conventions for Server Components, hooks, prop depth, and colocated tests. Use when editing React components.'
applyTo: '**/*.tsx, **/*.jsx'
---

# React

Apply these rules to React UI code.

- Default to Server Components. Add the `"use client"` directive only in a file that uses state, effects, event handlers, or browser-only APIs.
- Place `"use client"` on the leaf that needs it. Leave routes and layouts as Server Components when a single child needs the client.
- Call hooks at the top level of a component or of another hook. Follow the Rules of Hooks.
- Name custom hooks `useX`.
- Pass props through at most two component levels. From the third level, use context or the state library the project already uses.
- Colocate each component test with the component (`Component.test.tsx` beside `Component.tsx`).
