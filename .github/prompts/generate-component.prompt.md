---
description: 'Generate a UI component and its colocated test from the Angular or React instruction file.'
name: 'generate-component'
argument-hint: 'Component name and target path'
agent: 'agent'
---

Generate a component from the request that follows this prompt. If the component name or target path is missing, ask for it before writing files.

1. Choose the stack from the target path. Angular files are `.ts` and `.html`. React files are `.tsx` or `.jsx`.
2. Follow the matching instruction file. Apply those rules, and do not copy them into comments in the new files.
   - Angular: [angular.instructions.md](../instructions/angular.instructions.md)
   - React: [react.instructions.md](../instructions/react.instructions.md)
3. Also follow [accessibility.instructions.md](../instructions/accessibility.instructions.md) and [performance.instructions.md](../instructions/performance.instructions.md).
4. Add the component, the Angular template or React markup, and a colocated test that asserts the public behavior.
5. Stop once the new files satisfy those instruction files.
