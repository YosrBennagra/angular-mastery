# Senior Angular Question Bank

## TypeScript / foundations
1. Why does `http.get<User>()` not validate runtime JSON?
2. Use discriminated unions for async state.
3. Explain structural typing risks.
4. When is `as SomeType` a smell?
5. Where should domain logic live?

## Components/templates
6. Input/output vs shared state: choose boundaries.
7. When should a directive become a component?
8. Why can template methods be expensive?
9. Pure pipe vs computed state?
10. Design a reusable accessible component API.

## DI/routing
11. Explain hierarchical injectors.
12. Diagnose duplicate service instances.
13. Route-scoped vs root-scoped providers.
14. Guard vs resolver vs component loading.
15. Why does a guard not provide authorization?

## Forms/HTTP
16. Reactive vs template forms for dynamic onboarding.
17. Design cross-field validation.
18. Explain interceptor ordering.
19. Why is retrying POST dangerous?
20. Model server validation errors.

## RxJS
21. Pick switchMap/concatMap/mergeMap/exhaustMap for four workflows.
22. Explain cold vs shared HTTP streams.
23. Prevent duplicate requests.
24. Place error handling in long-lived streams.
25. Test debounce with virtual time.

## Signals/state
26. Computed vs effect.
27. Diagnose an effect feedback loop.
28. Design route-local feature state.
29. Decide whether NgRx is justified.
30. Bridge websocket Observables into signal state.

## Architecture/libraries
31. Refactor file-type folders into feature boundaries.
32. Shared vs core.
33. Facade: useful vs redundant.
34. Prevent deep feature imports.
35. Design a stable Angular library public API.

## Performance
36. Diagnose rerendering on every keypress.
37. Explain stable tracking for lists.
38. When can lazy loading hurt?
39. Profile network vs rendering bottlenecks.
40. Explain why premature memoization is harmful.

## Security/accessibility
41. Design auth session bootstrap.
42. Why is UI permission hiding not authorization?
43. Audit keyboard/focus handling.
44. Make form errors screen-reader discoverable.
45. Explain why frontend env values are not secrets.

## SSR/testing/debugging
46. Diagnose hydration mismatch from time/randomness.
47. Test router + providers at the right boundary.
48. Diagnose duplicate SSR/client HTTP requests.
49. Separate build-time and runtime configuration.
50. Perform a senior review of a mega-component and propose boundaries.
