---
name: migrate-to-signals
description: 'Migrates Angular local UI state from RxJS subjects to signals. Use when asked to convert a component or service to signals. Leaves HTTP and other real async streams on RxJS.'
---

# Migrate to signals

Migrate one Angular file at a time. This skill is self-contained: follow the rules below even when the repository instruction files are not in context.

## Rules

- Replace local UI state with `signal` and `computed`.
- Read signals by calling them in the template and in tests.
- Keep HTTP, websockets, and other real async streams as observables.
- Leave the component standalone and set `changeDetection: ChangeDetectionStrategy.OnPush`.
- Do not introduce NgRx while migrating. NgRx is only for state shared across features.
- Update the colocated `*.spec.ts` in the same change.

## Steps

1. Open the requested component or service. If no path was given, ask for one.
2. List every `Subject`, `BehaviorSubject`, and template-only field.
3. Mark each item as local state or as a real async stream.
4. Convert local state to `signal` or `computed`. Delete the subject and the subscriptions that only forwarded it.
5. Replace `async` pipes and `.subscribe()` calls that existed for that local state with signal reads.
6. Keep the async stream and its `async` pipe or `toSignal()` at the boundary where the stream enters the component.
7. Run the colocated unit test and fix assertions that still expect an observable.

## Example

Local state before:

```typescript
private readonly expandedSubject = new BehaviorSubject(false);
readonly expanded$ = this.expandedSubject.asObservable();

toggle(): void {
  this.expandedSubject.next(!this.expandedSubject.value);
}
```

```html
<button type="button" (click)="toggle()">
  {{ (expanded$ | async) ? 'Hide' : 'Show' }}
</button>
```

Local state after:

```typescript
protected readonly expanded = signal(false);

protected toggle(): void {
  this.expanded.update((open) => !open);
}
```

```html
<button type="button" (click)="toggle()" [attr.aria-expanded]="expanded()">
  {{ expanded() ? 'Hide' : 'Show' }}
</button>
```

Leave a real async stream on RxJS. `toSignal()` belongs at the edge, where the stream enters the component, and only when the template needs a signal.

```typescript
private readonly api = inject(InvoiceApi);

readonly invoice$ = this.api.getInvoice(this.id);
```

```html
@if (invoice$ | async; as invoice) {
  <p>{{ invoice.total }}</p>
}
```
