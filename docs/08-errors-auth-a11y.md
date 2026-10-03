# 08 — Error Handling, Authentication/Authorization Integration and Accessibility

## Wall Note / A4

- Errors need ownership: transport, application, page or field.
- Do not globally swallow errors.
- Authentication establishes identity/session; authorization decides allowed actions.
- Backend enforces authorization.
- Accessibility is a functional requirement, not polish.
- Native HTML semantics beat custom ARIA when possible.

## Detailed Notes

### Error handling

Classify:
- transport/network;
- authentication/session;
- authorization;
- validation;
- business conflict;
- unexpected application defect.

Global error handling is appropriate for unexpected/unhandled failures and telemetry. Expected feature errors should be represented in feature state and UI.

Do not turn every HTTP error into a generic toast.

### Authentication

Client responsibilities may include:
- bootstrap session state;
- attach credentials/token correctly;
- refresh/re-auth flows;
- redirect UX;
- clear sensitive local state on logout.

Avoid storing long-lived secrets unnecessarily in browser-accessible storage. Security model belongs in [application-security](https://github.com/YosrBennagra/application-security).

### Authorization

Client authorization can hide/disable controls, but users can modify client code and requests. Server/API authorization is authoritative.

### Accessibility

Start with:
- semantic landmarks/headings;
- labels for inputs;
- button vs link semantics;
- keyboard operation;
- focus management after dialogs/navigation;
- visible focus;
- status/error announcements where needed;
- sufficient target size/contrast;
- reduced-motion respect where relevant.

Use ARIA to fill semantic gaps, not to recreate native controls badly.

### Forms/accessibility

Error text should be associated with its input and discoverable by assistive technology. Do not communicate validity only through color.

## Practical Example

```html
<label for="email">Email</label>
<input
  id="email"
  type="email"
  [formControl]="email"
  [attr.aria-invalid]="email.invalid && email.touched"
  aria-describedby="email-error"
/>
@if (email.invalid && email.touched) {
  <p id="email-error" role="alert">Enter a valid email address.</p>
}
```

## Exercises / Senior Questions

1. Which errors belong globally vs locally?
2. Why is hiding an admin button not authorization?
3. Design refresh-token failure UX.
4. Audit a custom dropdown for keyboard/screen-reader behavior.
5. Why prefer native button/select/dialog semantics where possible?

## Related / Prerequisite Links

- [Application security](https://github.com/YosrBennagra/application-security)
- [API engineering](https://github.com/YosrBennagra/api-engineering)
