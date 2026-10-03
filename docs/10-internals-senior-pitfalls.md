# 10 — Angular Internals and Senior-Level Pitfalls

## Wall Note / A4

- Know framework internals enough to predict lifetime/rendering behavior.
- DI scope bugs often look like state bugs.
- RxJS flattening choice is concurrency policy.
- Effects can create hidden reactive cycles.
- Root singletons are global mutable state if misused.
- "Shared" and "generic" abstractions can increase coupling.
- Performance fixes without measurement are guesses.

## Detailed Notes

### Internals worth understanding

You do not need to memorize Angular source, but should understand:

- component/view creation and destruction;
- hierarchical injection;
- provider lifetime;
- template binding evaluation;
- reactive dependency tracking;
- router activation/lazy boundaries;
- subscription/resource cleanup;
- server vs browser execution;
- bundling/tree-shaking constraints.

### Senior pitfall: accidental singleton state

A root-provided service with mutable state can couple unrelated routes/users/workflows.

Fix by choosing ownership deliberately: component, route, feature, application.

### Senior pitfall: imperative RxJS nesting

Bad:

```ts
this.route.params.subscribe(p => {
  this.api.get(p['id']).subscribe(order => {
    this.order = order;
  });
});
```

Problems: nested lifecycle, stale requests, manual state, poor error composition.

Better:

```ts
readonly order$ = this.route.paramMap.pipe(
  map(p => p.get('id')!),
  distinctUntilChanged(),
  switchMap(id => this.api.get(id))
);
```

### Senior pitfall: effect as synchronization engine

Multiple effects writing to each other's signals create hidden temporal coupling. Prefer a single source of truth + computed derivations.

### Senior pitfall: mega components

Symptoms:
- hundreds of template lines;
- many unrelated services;
- multiple forms;
- routing/data/domain/UI logic together;
- broad state.

Split by responsibility and ownership, not merely by line count.

### Senior pitfall: generic abstraction too early

A configurable "universal table/form/page engine" can become harder than concrete features. Extract after stable duplication and shared semantics are visible.

### Senior pitfall: client security assumptions

Route guards, hidden buttons, obfuscated endpoints and client roles are not enforcement.

### Senior pitfall: incorrect optimization

Do not:
- convert everything to global state;
- memoize every value;
- lazy-load every component;
- eliminate RxJS indiscriminately;
- use manual change detection before profiling.

### Review checklist

For a feature PR ask:
1. Who owns state and lifetime?
2. Are async semantics explicit?
3. Is DI scope intentional?
4. Is navigation a first-class boundary?
5. Does UI preserve accessibility?
6. Can network/runtime errors be represented?
7. Is derived state computed rather than synchronized?
8. Are tests behavioral?
9. Is the feature independently lazy-loadable if it should be?
10. Are cross-feature dependencies controlled?

## Practical Example

A root `SelectionService` used by two unrelated pages leaks selection across navigation. Make it route-scoped via route `providers`, or keep selection directly in page/feature state if no sharing is needed.

## Exercises / Senior Questions

1. Explain a bug caused by provider scope.
2. Refactor nested subscriptions.
3. Diagnose an effect feedback loop.
4. Review a "generic CRUD framework" proposal.
5. Explain Angular architecture to a staff engineer without naming APIs first.

## Related / Prerequisite Links

- [Software architecture](https://github.com/YosrBennagra/software-architecture)
- [Programming principles](https://github.com/YosrBennagra/programming-principles)
- [Testing engineering](https://github.com/YosrBennagra/testing-engineering)
