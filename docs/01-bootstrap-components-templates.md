# 01 — Bootstrap, Standalone Components, Templates, Directives and Pipes

## Wall Note / A4

- Bootstrap defines root providers and application-wide behavior.
- Standalone components import what they use.
- Inputs = data in; outputs = events out.
- Templates should remain declarative.
- Directives add behavior; pipes transform presentation values.
- Avoid expensive functions in templates.

## Detailed Notes

### Bootstrap

Modern Angular applications commonly bootstrap with application-level providers rather than a root NgModule.

```ts
bootstrapApplication(AppComponent, {
  providers: [
    provideRouter(routes),
    provideHttpClient(withInterceptors([authInterceptor]))
  ]
});
```

Root providers are effectively application-lifetime dependencies. Do not place feature-specific mutable state there unless global lifetime is intended.

### Standalone components

Standalone components make dependencies explicit at the component/feature boundary.

```ts
@Component({
  standalone: true,
  selector: 'app-user-card',
  imports: [DatePipe],
  template: `
    <article>
      <h2>{{ user().name }}</h2>
      <time>{{ user().createdAt | date }}</time>
    </article>
  `
})
export class UserCardComponent {
  user = input.required<User>();
}
```

### Templates/bindings

Know:
- interpolation;
- property binding;
- event binding;
- two-way binding when appropriate;
- class/style binding;
- built-in control flow;
- template references;
- content projection.

Keep templates declarative. If an expression performs non-trivial computation, move it into computed state or a pure transformation.

### Component communication

Prefer:
- inputs for parent → child data;
- outputs for child → parent events;
- shared state service/store for siblings or cross-route state only when ownership warrants it;
- router params/state for navigation-owned state.

Avoid components reaching upward via injected parents unless implementing a deliberate compound-component pattern.

### Directives

Use directives for reusable DOM behavior that does not deserve its own visual component: focus management, permissions presentation, drag behavior, intersection observation, etc.

### Pipes

Pure pipes are appropriate for deterministic presentation transforms. Do not hide network calls, mutable state or expensive side effects in pipes.

## Practical Example

```ts
@Component({
  standalone: true,
  selector: 'app-counter',
  template: `
    <button type="button" (click)="decrement.emit()">−</button>
    <output>{{ value() }}</output>
    <button type="button" (click)="increment.emit()">+</button>
  `
})
export class CounterComponent {
  value = input.required<number>();
  increment = output<void>();
  decrement = output<void>();
}
```

The component is reusable because ownership of the actual state is explicit.

## Exercises / Senior Questions

1. What belongs in application bootstrap vs a feature provider?
2. When should a directive become a component?
3. Why can calling methods from templates cause performance surprises?
4. How do you choose between input/output and shared service state?
5. What makes a pipe unsuitable?

## Related / Prerequisite Links

- [TypeScript + mental model](00-typescript-angular-mental-model.md)
- [Feature architecture](06-architecture-libraries.md)
