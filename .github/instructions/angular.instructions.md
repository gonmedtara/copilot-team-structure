---
description: 'Angular conventions for standalone components, OnPush, signals, and state boundaries. Use when editing Angular TypeScript or templates.'
applyTo: '**/*.ts, **/*.html'
---

# Angular

Apply these rules to Angular application code.

- Generate standalone components. Register new components by import, not by `NgModule` declarations.
- Set `changeDetection: ChangeDetectionStrategy.OnPush` on every component.
- Keep local UI state in signals (`signal`, `computed`, `input`, `output`, `model`).
- Reserve RxJS for real async streams such as HTTP and websockets.
- Read signals in templates by calling them (`{{ total() }}`).
- Use NgRx only for state shared across features. Keep feature-local state in the component or a feature service.
- Split smart and dumb components. Smart components load data and call services. Dumb components take `input()` values and emit `output()` events.
- Keep services and the store out of dumb components.
