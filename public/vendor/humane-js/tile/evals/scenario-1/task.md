# Notification Manager

A simple notification manager for displaying user messages with appropriate timing and behavior.

## Capabilities

### Success Notifications

- It displays success messages for 2 seconds with automatic dismissal [@test](./notifier.test.js)
- It calls a callback function when the success notification completes [@test](./notifier.test.js)

### Error Notifications

- It displays error messages that require a click to close [@test](./notifier.test.js)
- It calls a callback function when the error notification is dismissed [@test](./notifier.test.js)
- It can override click-to-close for urgent errors that auto-dismiss after 3 seconds [@test](./notifier.test.js)

### Info Notifications

- It displays info messages for 4 seconds with automatic dismissal [@test](./notifier.test.js)
- It calls a callback function when the info notification completes [@test](./notifier.test.js)

## Implementation

[@generates](./notifier.js)

## API

```javascript { #api }
export function showSuccess(message, onComplete);
export function showError(message, onComplete, options);
export function showInfo(message, onComplete);
```

The `showError` function accepts an optional `options` parameter that may contain `{ autoDismiss: true }` to override the default click-to-close behavior.

## Dependencies { .dependencies }

### humane-js { .dependency }

Provides browser notification functionality.
