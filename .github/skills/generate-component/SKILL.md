---
name: generate-component
description: 'Generates an Angular standalone component or a React component with a colocated test. Use when asked to create, scaffold, or add a UI component.'
---

# Generate a component

Ask for the component name and target path when the request omits them. Create the component and a colocated test. This skill is self-contained: follow the rules below even when the repository instruction files are not in context.

## Angular

Use these rules for `.ts` and `.html` targets.

- Generate a standalone component with `changeDetection: ChangeDetectionStrategy.OnPush`.
- Keep local UI state in signals. Use `input()` and `output()` for the public API.
- Use RxJS only for a real async stream.
- Keep the component dumb unless the request asks it to load data. A dumb component does not inject services or a store.
- Put the spec next to the component as `*.spec.ts`.

## React

Use these rules for `.tsx` and `.jsx` targets.

- Keep the file a Server Component unless it needs state, effects, event handlers, or browser APIs. Add `"use client"` only on that file.
- Call hooks unconditionally at the top level.
- Stop prop passing at two levels. Use context or the project's state library past that.
- Put `Component.test.tsx` next to `Component.tsx`.

## Shared

- Use a native element (`button`, `a`, `label`) before adding ARIA.
- The control must work from the keyboard and expose an accessible name.
- Do not add a dependency for this component.

## Example

Angular dumb component:

```typescript
import { ChangeDetectionStrategy, Component, input, output, signal } from '@angular/core';

@Component({
  selector: 'app-invoice-summary',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  templateUrl: './invoice-summary.component.html',
})
export class InvoiceSummaryComponent {
  readonly total = input.required<number>();
  readonly toggle = output<boolean>();
  protected readonly expanded = signal(false);

  protected onToggle(): void {
    this.expanded.update((open) => !open);
    this.toggle.emit(this.expanded());
  }
}
```

```html
<section>
  <h2>Invoice</h2>
  <p>{{ total() }}</p>
  <button type="button" (click)="onToggle()" [attr.aria-expanded]="expanded()">
    Details
  </button>
</section>
```

React client leaf, used under a Server Component parent:

```tsx
'use client';

export function InvoiceSummary({ total }: { total: number }) {
  return (
    <section>
      <h2>Invoice</h2>
      <p>{total}</p>
    </section>
  );
}
```
