# Angular Mastery — Cheat Sheet

> Modern Angular (standalone, signals, control flow, zoneless). One-page wall note: [wall-notes/angular-a4.md](wall-notes/angular-a4.md). Hub: [software-engineer-roadmap](https://github.com/YosrBennagra/software-engineer-roadmap)

```
Bootstrap → DI/router → feature → state/streams → template → DOM
```

## Components & templates
| Thing | One line |
|---|---|
| Standalone component | imports what it uses (default since v19; NgModules are legacy) |
| `input()` / `output()` / `model()` | signal inputs, event outputs, two-way binding |
| Control flow | `@if`, `@for (x of xs; track x.id)`, `@switch`. `track` is **required** and uses a stable id |
| `@defer (on viewport)` | lazy-load a template region + its dependencies |
| Directive / Pipe | adds behaviour to elements / pure value formatting |
| Content projection | `<ng-content>`: the parent owns the projected content |
| `viewChild()` / `contentChild()` | signal queries: own view vs projected content |
- Data in, events out. Smart (container) vs presentational components.
- No heavy function calls in templates. Use `computed()` or pure pipes.

## Dependency injection
| Provider location | Lifetime |
|---|---|
| `providedIn: 'root'` | app-wide singleton = **global state** |
| Route `providers` | feature / lazy-route lifetime |
| Component `providers` | one instance per component subtree |
- `inject()` function. `InjectionToken` for config and abstractions.
- DI scope bugs look like state bugs ("why is the data shared?").

## Signals vs RxJS
| | Signals | RxJS |
|---|---|---|
| Models | synchronous **state** (current value) | **events over time**, async streams |
| Read | `count()` | `subscribe` / `async` pipe |
| Derive | `computed()` (pure, lazy, memoised) | `map`, `combineLatest` |
| Side effect | `effect()`. **Never** use it to sync derived state | `tap`, subscribe |
| Bridge | `toSignal(obs$)`, `toObservable(sig)` (`@angular/core/rxjs-interop`) | |
- `linkedSignal()` = writable state that resets when a source changes. `resource()` / `httpResource()` = async data as signals (value, status, error).
- One owner per piece of state. Don't mirror it in a store + a signal + a subject.

## RxJS flattening = concurrency policy
| Operator | Behaviour | Use for |
|---|---|---|
| `switchMap` | cancel the previous inner, **latest wins** | search/typeahead, route params |
| `concatMap` | queue, **in order** | ordered writes/saves |
| `mergeMap` | all **in parallel** | independent requests (limit concurrency) |
| `exhaustMap` | **ignore** new while busy | login/submit button, polling |
- Other key operators: `debounceTime`, `distinctUntilChanged`, `catchError`, `retry({count, delay})`, `shareReplay({bufferSize:1, refCount:true})`, `takeUntilDestroyed()`.
- `HttpClient` observables are **cold**: every subscribe = a new request. Complete after one emission.
- Cold = per-subscriber execution. Hot/shared = one execution, many subscribers (`share`, `Subject`).
- Subject types: `Subject` (no value) · `BehaviorSubject` (current value) · `ReplaySubject(n)` · `AsyncSubject` (last value on complete).

## Change detection & performance
- Zone.js triggered CD after any async event. **Zoneless** (stable v20.2, default for new apps in v21) → CD runs on signal changes, template events, `async` pipe, `markForCheck`.
- `OnPush`: check only on input reference change / event / async pipe / signal read in the template / `markForCheck()`. **Default for components since v22**; `ChangeDetectionStrategy.Eager` = the old `Default` (opt-out while migrating).
- Mutating an object in place under OnPush = no update → use immutable updates.
- Lazy routes `loadComponent` / `loadChildren`, plus `@defer` for heavy regions.
- Measure: bundle (`ng build --stats-json`), Angular DevTools profiler, Lighthouse/Web Vitals (LCP, INP, CLS).

## Routing
- Routes = architecture boundaries. Lazy-load per feature.
- Functional guards (`CanActivateFn`) / resolvers. **Guards are UX, not security**: the backend authorises.
- `withComponentInputBinding()` → route params arrive as inputs. v22: child routes inherit all parent params by default (`paramsInheritanceStrategy: 'always'`).

## Forms & HTTP
| Form model | When |
|---|---|
| Signal Forms (`@angular/forms/signals`, stable v22) | new signal-based apps: typed model + schema validation |
| Reactive forms | complex/dynamic forms, existing Observable code |
| Template-driven | genuinely simple forms |
- Client validation = UX. **Server validation is authoritative.**
- Functional interceptors (`HttpInterceptorFn`): auth header, errors, retry, loading. **Order matters.**
- v22: the `fetch` backend is the default (`withFetch()` deprecated); use `withXhr()` only if you need upload progress.
- `HttpClient<T>` typing is not runtime validation.

## Lifecycle
`constructor` (DI only) → `ngOnChanges` → `ngOnInit` → `ngAfterViewInit` → `ngOnDestroy`. Prefer signals + `computed`; clean up with `DestroyRef`/`takeUntilDestroyed()`; DOM work in `afterNextRender`.

## Architecture, security, a11y
- Feature-first folders (routes + UI + state + data access); `shared` = generic UI, `core` = app singletons. Facade between components and state/API. State: local signals → signal service → NgRx SignalStore/Store only when justified. Lint-enforced library boundaries (Nx).
- Interpolation is auto-escaped; `bypassSecurityTrust*`/`innerHTML` with user data = XSS. **No secrets in the bundle** (`environment.ts` is public). Prefer HttpOnly cookies (+ CSRF) or a BFF over `localStorage` tokens.
- a11y: semantic HTML, labels, focus management, keyboard navigation.

## SSR & hydration
- Server code runs per request: no `window`/`document` without guards (`isPlatformBrowser`, `afterNextRender`).
- Hydration reuses server DOM, so server and client output must match. Incremental hydration = `@defer` + `hydrate on …` triggers (on by default since v22).

## Testing
- Test user-visible behaviour (TestBed + component harness / Testing Library), not private fields. **Vitest** is the default runner for new projects since v21 (Karma still supported).
- Use `HttpTestingController` for HTTP. Use E2E (Playwright/Cypress) for critical journeys only.

## Senior gotchas
- `effect()` writing signals it reads (use `computed`/`linkedSignal`) · `subscribe` inside `subscribe` · `mergeMap` on saves · `shareReplay` without `refCount`.
- Root service holding per-user state under SSR (leaks between requests) · `track $index` on reorderable lists.

**Senior review:** Who owns this state? Is the lifetime right? Is the async operator right? Any hidden effect cycle? Is cleanup owned? Any cross-feature coupling? Accessible? Measured?
