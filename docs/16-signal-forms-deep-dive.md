# 16 — Signal Forms and Modern Forms Architecture

## Wall Note / A4

- Signal Forms use a writable signal as the form-model source of truth.
- The form builds a typed field tree around that model.
- Validation/field behavior belongs in schemas, not scattered template conditions.
- Client validation improves UX; server validation remains authoritative.
- Signal Forms are stable in Angular v22; verify availability for older projects.
- Reactive forms remain a strong choice for established Observable-heavy forms.
- Template-driven forms are appropriate for genuinely simple local forms.
- Pick one primary form model per feature; interoperability is for migration/integration, not architectural indecision.

## Detailed Notes

### 1. The three form approaches

#### Signal Forms

Strong fit when:
- application already uses signals heavily;
- typed model-first form state is desirable;
- schema-driven validation/field logic improves clarity;
- new Angular code can target a version supporting stable Signal Forms.

#### Reactive Forms

Strong fit when:
- existing large codebase already uses `FormGroup` / `FormControl`;
- libraries and team patterns depend on reactive-form APIs;
- Observable-based form pipelines are mature;
- migration cost would outweigh benefits.

#### Template-driven forms

Strong fit when:
- form is small;
- state is local;
- validation is straightforward;
- explicit programmatic form graph would add more code than value.

Do not classify one approach as universally "senior." Seniority is choosing based on constraints.

### 2. Signal Forms model

```mermaid
flowchart LR
    M[Writable signal model] --> F[form()]
    F --> T[Typed FieldTree]
    S[Schema rules] --> T
    T --> UI[FormField bindings]
    UI --> M
    M --> V[Application state / submit payload]
```

The model signal remains the data source of truth.

### 3. Basic form

```ts
interface LoginModel {
  email: string;
  password: string;
}

readonly loginModel = signal<LoginModel>({
  email: '',
  password: ''
});

readonly loginForm = form(this.loginModel, path => {
  required(path.email);
  email(path.email);
  required(path.password);
  minLength(path.password, 12);
});
```

Template concept:

```html
<input type="email" [formField]="loginForm.email" />
<input type="password" [formField]="loginForm.password" />
```

The field tree exposes form/field state while changes synchronize with the model.

### 4. Schema-driven logic

Keep field logic together:

- required/optional conditions;
- disabled state;
- readonly state;
- hidden state;
- debounce behavior;
- validation;
- metadata for custom controls.

This avoids sprinkling duplicated conditions across templates and submission code.

### 5. Cross-field validation

Cross-field rules should express a real domain/form relationship:

```ts
readonly model = signal({
  password: '',
  confirmPassword: ''
});

readonly passwordForm = form(this.model, path => {
  required(path.password);
  required(path.confirmPassword);

  validate(path.confirmPassword, ctx =>
    ctx.value() === this.model().password
      ? undefined
      : { kind: 'passwordMismatch' }
  );
});
```

If the rule is an authoritative business rule requiring server state, keep the server authoritative and surface the server result.

### 6. Submission

A good submission flow handles:
- touch/error visibility;
- client validation;
- duplicate submission;
- async request state;
- server field errors;
- global/non-field errors.

Signal Forms provide submission APIs that can integrate returned validation errors into fields.

Do not build a second disconnected `isSubmitting` state if the form API already exposes the needed lifecycle.

### 7. Server errors

Map errors intentionally:

**Field error**
- email already in use;
- invalid postal code.

**Form/global error**
- payment provider unavailable;
- optimistic concurrency conflict;
- permission revoked.

Do not attach every server failure to the first field just to display it.

### 8. Custom controls

A reusable form control must have:
- clear value contract;
- accessible label/description/error semantics;
- disabled/readonly handling;
- focus behavior;
- touch/dirty behavior as required.

Modern Signal Forms support custom control contracts. Existing ControlValueAccessor-based components remain relevant for compatibility.

Do not build custom controls when a native input/select/textarea already satisfies the requirement.

### 9. Migration from reactive forms

Do not rewrite all forms at once.

Good migration:
1. new isolated feature adopts Signal Forms;
2. shared custom controls gain compatible contracts where needed;
3. measure developer/testing benefits;
4. migrate stable boundaries opportunistically;
5. use compatibility bridges only where integration requires them.

Avoid hybrid state where the same field is independently owned by both a signal model and a `FormControl`.

### 10. Form architecture

Separate:
- **form model** — editable representation;
- **domain/server DTO** — transport/business representation;
- **view-only state** — expansion, focused section, dialog state;
- **submission state/errors**.

Do not force backend DTO shape to become the ideal user-editing model.

Example:

```ts
interface AddressFormModel {
  street: string;
  city: string;
  postalCode: string;
}

function toUpdateAddressCommand(model: AddressFormModel): UpdateAddressCommand {
  return {
    street: model.street.trim(),
    city: model.city.trim(),
    postalCode: normalizePostalCode(model.postalCode)
  };
}
```

Mapping at the boundary keeps UI concerns from leaking into API models.

### 11. Async validation

Use asynchronous validation for checks that are truly remote:
- username availability;
- server-backed lookup.

Avoid firing a request on every keystroke without debounce/cancellation policy.

Also remember: an availability check can race with final submission. The server must validate again atomically where required.

### 12. Accessibility

Every form approach must still deliver:
- labels;
- described errors;
- keyboard operation;
- correct input type/autocomplete;
- focus strategy after failed submission;
- clear required/optional semantics;
- non-color-only error indication.

Framework state APIs do not guarantee accessible markup.

### 13. Testing

Test:
- schema rules;
- conditional disabled/hidden behavior;
- cross-field rules;
- submission state;
- server-error mapping;
- rendered accessibility/interaction.

Do not test Angular's own form library internals.

## Practical Example — profile form

```ts
interface ProfileFormModel {
  displayName: string;
  email: string;
  marketing: boolean;
}

readonly model = signal<ProfileFormModel>({
  displayName: '',
  email: '',
  marketing: false
});

readonly profileForm = form(this.model, path => {
  required(path.displayName);
  minLength(path.displayName, 2);

  required(path.email);
  email(path.email);
});

async save() {
  const value = this.model();

  await this.api.updateProfile({
    displayName: value.displayName.trim(),
    email: value.email.trim().toLowerCase(),
    marketing: value.marketing
  });
}
```

The form model is explicit, validation is centralized and API mapping stays at the boundary.

## Practical Example — choose, do not mix blindly

Existing enterprise feature:
- 70 reactive forms;
- mature custom CVA library;
- extensive RxJS form workflows;
- stable tests.

Recommendation: keep reactive forms; use Signal Forms for new isolated features only after validating interoperability.

Greenfield Angular v22+ application:
- signal-first feature state;
- new component library;
- typed form-model architecture.

Recommendation: Signal Forms are a strong default candidate.

## Failure Modes

- Adopting Signal Forms solely because they are newer.
- Duplicating field state in `FormControl` and writable signals.
- Performing authoritative business validation only in the browser.
- Remote validator with no debounce/cancellation.
- Binding API DTOs directly to complex editable UI.
- Custom control ignores accessibility/focus/disabled state.
- Server errors displayed only as generic toast.
- Massive one-shot migration from working reactive forms.

## Exercises / Senior Questions

1. Choose a forms approach for three applications with different constraints.
2. Model a checkout form separately from its API command.
3. Design a schema with conditional required/disabled fields.
4. Explain why remote uniqueness validation must still run on submit/server.
5. Plan a gradual reactive-forms → Signal Forms migration.
6. Design a custom date-range control contract with accessibility requirements.
7. Test server field-error mapping without duplicating backend logic.

## Related / Prerequisite Links

- [Forms/HTTP core](03-forms-http.md)
- [Signals/state deep dive](12-rxjs-signals-state-deep-dive.md)
- [Accessibility/auth/errors](08-errors-auth-a11y.md)
- [Testing engineering](https://github.com/YosrBennagra/testing-engineering)
- [Angular Signal Forms overview](https://angular.dev/guide/forms/signals/overview)
- [Angular Signal Forms API](https://angular.dev/api/forms/signals/form)
