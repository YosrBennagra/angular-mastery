# 07 — Change Detection, Rendering, Performance, Lazy Loading and Code Splitting

## Wall Note / A4

- Rendering cost comes from work + frequency + DOM size.
- Stable state boundaries reduce unnecessary checks.
- Signals make reactive dependencies explicit.
- Track lists by stable identity.
- Lazy loading is architectural splitting, not random fragmentation.
- Measure bundle, network, rendering and memory separately.

## Detailed Notes

### Rendering/change detection mental model

A state/event change makes Angular determine which views need refreshed bindings.

```mermaid
flowchart LR
    E[Event / async completion / signal write] --> S[State change]
    S --> CD[Angular schedules view work]
    CD --> B[Evaluate affected bindings]
    B --> DOM[Update DOM where values changed]
```

Optimization is about reducing:
- how often work is scheduled;
- how much view/binding work is considered;
- expensive computations;
- DOM churn.

### Signals and OnPush-style thinking

Even when framework defaults evolve, the senior principle remains: components should depend on explicit immutable/reactive inputs rather than invisible mutation.

Avoid mutating an object in place and expecting every consumer to infer semantic change.

### Lists

Stable identity matters.

```html
@for (order of orders(); track order.id) {
  <app-order-row [order]="order" />
}
```

Tracking by stable IDs avoids unnecessary DOM recreation.

### Template cost

Avoid:
- expensive methods called repeatedly from templates;
- getters that allocate arrays/objects;
- repeated sorting/filtering in template expressions;
- giant components with broad reactive dependencies.

Prefer computed/selectors and pre-shaped view models.

### Lazy loading and code splitting

Lazy load by user/navigation capability:
- admin;
- reports;
- rarely visited configuration;
- large feature areas.

Do not split tiny code solely to increase chunk count. Every lazy boundary adds requests/loading/error states and architectural complexity.

### Performance diagnosis

Measure:
- initial JS/CSS payload;
- route chunks;
- request waterfalls;
- CPU scripting/rendering;
- long tasks;
- memory/retained subscriptions;
- DOM size;
- repeated HTTP requests.

Use browser performance tools and Angular-aware profiling where available.

## Practical Example

Bad:
```html
<li *ngFor="let item of getFilteredAndSortedItems()">
```

Better:
```ts
readonly visibleItems = computed(() =>
  sortItems(filterItems(this.items(), this.filter()))
);
```

Then render the computed state with stable tracking.

## Exercises / Senior Questions

1. Diagnose a page that rerenders during every keystroke.
2. Why does stable list identity matter?
3. When does lazy loading worsen UX?
4. How would you separate network vs rendering bottlenecks?
5. Why can "memoize everything" be counterproductive?

## Related / Prerequisite Links

- [System design](https://github.com/YosrBennagra/system-design)
- [DevOps/platform engineering](https://github.com/YosrBennagra/devops-platform-engineering)
