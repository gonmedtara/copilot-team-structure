---
description: 'Migrate Angular local state from RxJS subjects to signals, leaving real async streams on RxJS.'
name: 'migrate-to-signals'
argument-hint: 'Component or service path to migrate'
agent: 'agent'
---

Migrate the named Angular file to signals. If no path is given, ask which file to change.

1. Follow [angular.instructions.md](../instructions/angular.instructions.md). Apply those rules, and do not copy them into comments.
2. Find local state held in `BehaviorSubject`, `Subject`, or fields that exist only to drive the template.
3. Replace that local state with `signal` or `computed`. Call the signal from the template. Remove `async` pipes that only unwrapped the replaced state.
4. Leave HTTP, websockets, and other real async streams on RxJS.
5. Keep the component standalone and on `ChangeDetectionStrategy.OnPush`.
6. Update the colocated spec so it reads signal values.
7. Do not add NgRx as part of this migration.
