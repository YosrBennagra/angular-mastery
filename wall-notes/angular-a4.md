# Angular Mastery — A4 Wall Note

## Mental model

**Bootstrap → DI/router → feature → state/streams → template → DOM**

### Components
- clear responsibility;
- data in, events out;
- declarative template;
- avoid business logic in UI.

### DI
- scope = lifetime;
- root only for true app-wide dependencies;
- route/component providers for local lifetime;
- use tokens for abstractions/config;
- avoid service-locator patterns.

### Routing
- routes are architecture boundaries;
- lazy-load features;
- guards = navigation UX, not security;
- resolver only when blocking navigation is intentional.

### Forms / HTTP
- reactive forms for complex explicit state;
- server validation is authoritative;
- HttpClient typing ≠ runtime validation;
- interceptor chain order matters;
- retries must respect operation semantics.

### RxJS
- switchMap = latest wins;
- concatMap = ordered queue;
- mergeMap = concurrency;
- exhaustMap = ignore while busy.

### Signals
- signal = state;
- computed = derived state;
- effect = side effect;
- never synchronize derivable state with effects.

### State
Local/feature state first. NgRx only when shared/event-driven complexity, tracing and team conventions justify it.

### Architecture
Feature-first. Keep `shared` generic. Prevent deep cross-feature imports. Stable public APIs for libraries.

### Performance
Stable identity. Avoid expensive template work. Measure bundle/network/CPU/DOM/memory separately. Lazy-load by capability, not fashion.

### Security/accessibility
Backend enforces authorization. Frontend secrets do not exist. Use semantic HTML, keyboard/focus behavior and accessible validation.

### SSR
No unconditional browser APIs. Keep request state isolated. Hydration requires compatible server/client output.

### Senior review
Who owns state? Correct lifetime? Correct async operator? Any hidden effect cycle? Cross-feature coupling? Accessible? Testable? Measured?
