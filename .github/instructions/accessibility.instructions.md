---
description: 'WCAG 2.1 AA baseline for semantic HTML, ARIA, keyboard access, and contrast. Use when editing React or HTML templates.'
applyTo: '**/*.tsx, **/*.html'
---

# Accessibility

Target WCAG 2.1 level AA.

- Use semantic HTML (`button`, `a`, `nav`, `main`, `label`, headings in order) for the control's role.
- Add ARIA only when the native element cannot express the role, name, or state.
- Make every interactive control reachable and operable with the keyboard alone.
- Keep a visible focus indicator. A removed outline needs a replacement that meets contrast.
- Associate each input with a visible `<label>`. Placeholder text is not a label.
- Meet contrast of 4.5:1 for normal text and 3:1 for large text and meaningful UI boundaries.
- Do not use color as the only way to convey state.
