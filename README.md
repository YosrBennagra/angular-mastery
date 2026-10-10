# Angular Mastery — 0 → Expert

> **Cheat sheet:** [CHEAT-SHEET.md](CHEAT-SHEET.md) (dense one-to-two-page revision sheet to print and keep on the wall)

A dense Angular knowledge base from required TypeScript fundamentals through application architecture, reactivity, performance, SSR/hydration and senior-level internals.

> Master index: [software-engineer-roadmap](https://github.com/YosrBennagra/software-engineer-roadmap)

## Learning order

1. [TypeScript + Angular mental model](docs/00-typescript-angular-mental-model.md)
2. [Bootstrap, standalone components, templates, directives and pipes](docs/01-bootstrap-components-templates.md)
3. [Communication, DI, routing and guards](docs/02-communication-di-routing.md)
4. [Forms (including Signal Forms), HTTP and interceptors](docs/03-forms-http.md)
5. [RxJS and reactive programming](docs/04-rxjs.md)
6. [Signals, computed state, effects and state management](docs/05-signals-state.md)
7. [Feature architecture, libraries and reusable UI](docs/06-architecture-libraries.md)
8. [Change detection, rendering, performance and lazy loading](docs/07-rendering-performance.md)
9. [Errors, auth, authorization and accessibility](docs/08-errors-auth-a11y.md)
10. [Testing, SSR/hydration, config, debugging and profiling](docs/09-testing-ssr-config-debug.md)
11. [Angular internals and senior pitfalls](docs/10-internals-senior-pitfalls.md)

Use the [A4 wall note](wall-notes/angular-a4.md) for fast recall and the [senior question bank](exercises/senior-question-bank.md) for review.

## Topic ownership

This repository explains Angular-specific mechanics and decisions. It deliberately cross-links instead of duplicating broader subjects:

| Topic | Angular-specific scope here | Deep owner |
|---|---|---|
| Testing | TestBed/component/router/HTTP Angular mechanics | [testing-engineering](https://github.com/YosrBennagra/testing-engineering) |
| API design | HttpClient consumption/interceptors/errors | [api-engineering](https://github.com/YosrBennagra/api-engineering) |
| Architecture | Angular feature boundaries/composition | [software-architecture](https://github.com/YosrBennagra/software-architecture) |
| Principles | cohesion, coupling, SOLID, immutability | [programming-principles](https://github.com/YosrBennagra/programming-principles) |
| Security | Angular auth integration/client-side concerns | [application-security](https://github.com/YosrBennagra/application-security) |
| CI/build | Angular build/runtime configuration concepts | [devops-platform-engineering](https://github.com/YosrBennagra/devops-platform-engineering) |

## Progress checklist

- [ ] Use TypeScript types/generics/unions safely in Angular.
- [ ] Explain bootstrap, standalone APIs and provider scope.
- [ ] Build accessible components with clean inputs/outputs.
- [ ] Use directives/pipes appropriately.
- [ ] Design DI tokens/factories without service-locator behavior.
- [ ] Model routing, lazy routes, resolvers and guards correctly.
- [ ] Choose Signal Forms, reactive forms or template forms intentionally.
- [ ] Build typed HTTP flows and interceptor chains.
- [ ] Use RxJS operators by semantic intent.
- [ ] Use signals/computed/effects without hidden feedback loops.
- [ ] Know when local state is enough and when NgRx is justified.
- [ ] Structure features around business capabilities.
- [ ] Build reusable libraries without accidental global coupling.
- [ ] Explain change detection/rendering and avoid unnecessary work.
- [ ] Apply lazy loading/code splitting based on real boundaries.
- [ ] Integrate error handling, authentication and authorization safely.
- [ ] Build accessible keyboard/screen-reader-friendly interfaces.
- [ ] Test behavior at appropriate Angular boundaries.
- [ ] Explain SSR/hydration constraints.
- [ ] Separate build-time from runtime configuration.
- [ ] Debug change detection, RxJS, network and performance problems.
- [ ] Recognize senior-level Angular anti-patterns.

## Repository principles

- Prefer standalone, feature-oriented composition over accidental module/global coupling.
- Prefer explicit state ownership and one-way data flow.
- Use RxJS for streams/events/time and signals for synchronous reactive state; combine them deliberately.
- Effects are for side effects, not derived state.
- Client-side authorization is UX, never the security boundary.
- Optimize only after understanding rendering/network/bundle costs.
- Framework APIs are tools; application boundaries matter more than API fashion.


## Expert deep dives

After the core sequence, use these production-level chapters:

12. [DI, provider scopes and routing lifetimes](docs/11-di-routing-provider-lifetimes-deep-dive.md)
13. [RxJS, signals and state concurrency](docs/12-rxjs-signals-state-deep-dive.md)
14. [Rendering, zoneless change detection and performance](docs/13-rendering-zoneless-performance-deep-dive.md)
15. [SSR, hydration, runtime configuration and Angular testing](docs/14-ssr-hydration-testing-runtime-deep-dive.md)
16. [Component lifecycle, queries and composition](docs/15-component-lifecycle-composition-deep-dive.md)
17. [Signal Forms and modern forms architecture](docs/16-signal-forms-deep-dive.md)

These chapters revisit the core material at senior/expert depth: lifecycle ownership, injector resolution, asynchronous cancellation, signal/RxJS boundaries, modern rendering behavior, deferred loading, component composition, Signal Forms, server/client execution, hydration safety and test architecture.
