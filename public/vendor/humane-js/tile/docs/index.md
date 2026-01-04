# humane.js

humane.js is a framework-independent JavaScript notification library that displays unobtrusive, customizable browser messages to users. It provides CSS3 transition-based animations with automatic fallback to JavaScript animations for older browsers. The library supports multiple themes, mobile devices, and offers both a simple API for basic notifications and advanced features for queued messages, custom timing, and user interaction controls.

## Package Information

- **Package Name**: humane-js
- **Package Type**: bower
- **Language**: JavaScript
- **Installation**: `bower install humane-js`

## Core Imports

For browser global usage:

```javascript
// Include humane.min.js in your HTML
// Access via the global humane object
humane.log('Hello World');
```

For AMD (RequireJS):

```javascript
require(['humane'], function(humane) {
  humane.log('Hello World');
});
```

For CommonJS/Browserify:

```javascript
var humane = require('humane-js');
humane.log('Hello World');
```

## Basic Usage

```javascript
// Include a theme CSS file in your HTML:
// <link rel="stylesheet" href="themes/libnotify.css">
// <script src="humane.min.js"></script>

// Simple notification
humane.log('Welcome Back');

// Notification with HTML
humane.log('Record <b>392</b> has been updated');

// Notification with list (array automatically converted to HTML list)
humane.log(['First item', 'Second item', 'Third item']);

// Notification with callback
humane.log('Processing complete', function() {
  console.log('Notification finished');
});

// Notification with custom options
humane.log('Error occurred', {
  timeout: 4000,
  clickToClose: true,
  addnCls: 'humane-error'
});
```

## Architecture

humane.js uses a queue-based notification system with the following key components:

- **Default Instance**: A pre-instantiated Humane object exported as the default, ready for immediate use
- **Queue System**: Notifications are queued and displayed sequentially, ensuring messages don't overlap
- **Animation Engine**: Automatically detects CSS transition support and falls back to JavaScript-based opacity animation for older browsers (IE7-8)
- **Event System**: Cross-browser event handling with feature detection for addEventListener/attachEvent
- **Theme System**: CSS-based themes control visual appearance and animations independently of the JavaScript API

## Capabilities

### Display Notification

Display a notification message with optional configuration and callback.

```javascript { .api }
/**
 * Display a notification message
 * @param html - Message content (string or array of strings)
 * @param options - Configuration options or callback function
 * @param callback - Function called when notification completes
 * @returns The Humane instance for method chaining
 */
humane.log(html: string | string[], options?: HumaneOptions | Function, callback?: Function): Humane;

interface HumaneOptions {
  /** Duration in milliseconds before notification fades (0 for no timeout) */
  timeout?: number;
  /** Wait for user interaction before removing notification */
  waitForMove?: boolean;
  /** Allow clicking/touching notification to close it */
  clickToClose?: boolean;
  /** Delay in milliseconds after user interaction before hiding */
  timeoutAfterMove?: number;
  /** Additional CSS class(es) to apply to notification */
  addnCls?: string;
  /** Base CSS class for notification element */
  baseCls?: string;
}
```

**Usage Examples:**

```javascript
// Basic notification
humane.log('Operation successful');

// With callback (options parameter can be a function)
humane.log('Saving...', function() {
  console.log('Notification closed');
});

// With options object
humane.log('Important message', {
  timeout: 5000,
  clickToClose: true
});

// With both options and callback
humane.log('File uploaded', {
  timeout: 3000,
  addnCls: 'humane-success'
}, function() {
  console.log('Upload notification closed');
});

// Array becomes an unordered list
humane.log(['Error 1: Invalid input', 'Error 2: Missing field', 'Error 3: Network timeout']);
```

### Create Custom Instance

Create a new independent Humane instance with its own configuration and notification queue.

```javascript { .api }
/**
 * Create a new Humane instance
 * @param options - Configuration options for the new instance
 * @returns A new independent Humane instance
 */
humane.create(options?: HumaneOptions): Humane;
```

**Usage Examples:**

```javascript
// Create instance with custom defaults
var errorNotifier = humane.create({
  timeout: 4000,
  baseCls: 'humane-bigbox',
  addnCls: 'humane-bigbox-error'
});

errorNotifier.log('An error occurred');

// Create instance with custom container
var customNotifier = humane.create({
  container: document.getElementById('notification-area'),
  timeout: 3000
});

customNotifier.log('Custom positioned notification');

// Multiple independent instances
var topNotifier = humane.create({ baseCls: 'humane-libnotify' });
var bottomNotifier = humane.create({ baseCls: 'humane-bigbox' });

topNotifier.log('Top notification');
bottomNotifier.log('Bottom notification');
```

### Create Themed Notifier Function

Create a specialized notification function with predefined default options. Useful for creating semantic notification methods like `error()`, `info()`, or `success()`.

```javascript { .api }
/**
 * Create a notification function with default options
 * @param defaults - Default options to merge with each notification
 * @returns Function with signature (html, options, callback) that logs with merged defaults
 */
humane.spawn(defaults: HumaneOptions): (html: string | string[], options?: HumaneOptions | Function, callback?: Function) => Humane;
```

**Usage Examples:**

```javascript
// Create semantic notification methods
humane.error = humane.spawn({
  addnCls: 'humane-libnotify-error',
  timeout: 4000
});

humane.success = humane.spawn({
  addnCls: 'humane-libnotify-success',
  timeout: 2000
});

humane.info = humane.spawn({
  addnCls: 'humane-libnotify-info',
  timeout: 2500
});

// Use the spawned functions
humane.error('Failed to save');
humane.success('Saved successfully');
humane.info('Processing...');

// Spawned functions still accept options
humane.error('Critical error', { timeout: 0, clickToClose: true });

// Use on custom instances
var bigbox = humane.create({ baseCls: 'humane-bigbox' });
bigbox.error = bigbox.spawn({ addnCls: 'humane-bigbox-error' });
bigbox.error('Custom instance error');
```

### Remove Current Notification

Force the current notification to close immediately, optionally executing a callback when removal is complete.

```javascript { .api }
/**
 * Force remove the current notification
 * @param callback - Optional function called when removal completes
 */
humane.remove(callback?: Function): void;
```

**Usage Examples:**

```javascript
// Display a notification
humane.log('Processing...', { timeout: 0 }); // No automatic timeout

// Later, remove it programmatically
humane.remove();

// Remove with callback
humane.remove(function() {
  console.log('Notification removed');
  humane.log('Process complete');
});

// Useful for canceling notifications
var cancelButton = document.getElementById('cancel');
cancelButton.addEventListener('click', function() {
  humane.remove(function() {
    humane.log('Operation canceled');
  });
});
```

## Configuration

All configuration properties can be set on a Humane instance or passed as options to individual notifications. When passed to `log()`, options override the instance defaults for that notification only.

### timeout

Duration in milliseconds before the notification starts to fade out.

```javascript { .api }
humane.timeout = 2500; // Default value
```

**Usage Examples:**

```javascript
// Set globally for all notifications
humane.timeout = 5000;
humane.log('This will show for 5 seconds');

// Override for specific notification
humane.log('This will show for 10 seconds', { timeout: 10000 });

// Disable timeout (notification persists until user action)
humane.log('Click to close', { timeout: 0, clickToClose: true });
```

### waitForMove

When `true`, the notification will wait for user interaction (mouse move, click, keypress, or touch) before starting the removal process after the timeout expires.

```javascript { .api }
humane.waitForMove = false; // Default value
```

**Usage Examples:**

```javascript
// Enable globally
humane.waitForMove = true;
humane.log('This will wait for you to move');

// Use with timeout: message shows for 2.5s, then waits for user to move
humane.timeout = 2500;
humane.waitForMove = true;
humane.log('Read me first');

// Override for specific notification
humane.log('Important message', { waitForMove: true, timeout: 1000 });
```

### clickToClose

When `true`, clicking or touching the notification will immediately close it.

```javascript { .api }
humane.clickToClose = false; // Default value
```

**Usage Examples:**

```javascript
// Enable globally
humane.clickToClose = true;
humane.log('Click me to close');

// Enable for specific notification
humane.log('Click to dismiss', { clickToClose: true, timeout: 0 });

// Combine with other options
humane.log('Error details here', {
  clickToClose: true,
  timeout: 8000,
  addnCls: 'humane-error'
});
```

### timeoutAfterMove

Additional delay in milliseconds after user interaction before the notification is removed. Only applies when `waitForMove` is `true`.

```javascript { .api }
humane.timeoutAfterMove = 0; // Default value (false or 0)
```

**Usage Examples:**

```javascript
// Wait for user interaction, then show for 2 more seconds
humane.waitForMove = true;
humane.timeoutAfterMove = 2000;
humane.log('Read me when ready');

// Per-notification override
humane.log('Important update', {
  waitForMove: true,
  timeoutAfterMove: 3000,
  timeout: 1000
});

// Workflow: notification appears -> waits 1000ms (timeout) ->
// waits for user to move -> shows for 3000ms more -> fades out
```

### addnCls

Additional CSS class(es) to apply to the notification element. Useful for styling variations without changing the base theme.

```javascript { .api }
humane.addnCls = ''; // Default value
```

**Usage Examples:**

```javascript
// Set globally
humane.addnCls = 'humane-info';
humane.log('Info message');

// Override per notification
humane.log('Success message', { addnCls: 'humane-success' });
humane.log('Error message', { addnCls: 'humane-error' });
humane.log('Warning message', { addnCls: 'humane-warning' });

// Multiple classes
humane.log('Custom styled', { addnCls: 'custom-class another-class' });

// Use with spawn for semantic methods
humane.error = humane.spawn({ addnCls: 'humane-libnotify-error' });
humane.success = humane.spawn({ addnCls: 'humane-libnotify-success' });
```

### baseCls

Base CSS class applied to the notification element. Changing this switches the entire theme.

```javascript { .api }
humane.baseCls = 'humane'; // Default value
```

**Usage Examples:**

```javascript
// Switch theme globally
humane.baseCls = 'humane-libnotify';
humane.log('LibNotify theme');

// Switch theme dynamically
var themeSelector = document.getElementById('theme-select');
themeSelector.addEventListener('change', function() {
  humane.baseCls = 'humane-' + this.value;
  humane.log('Theme changed to ' + this.value);
});

// Different themes for different instances
var topNotify = humane.create({ baseCls: 'humane-libnotify' });
var bottomNotify = humane.create({ baseCls: 'humane-bigbox' });

topNotify.log('LibNotify styled');
bottomNotify.log('BigBox styled');

// Available built-in themes:
// - humane-libnotify
// - humane-bigbox
// - humane-boldlight
// - humane-jackedup
// - humane-original
// - humane-flatty
```

### container

DOM element to which the notification element will be appended. Defaults to `document.body`.

```javascript { .api }
humane.container = document.body; // Default value
```

**Usage Examples:**

```javascript
// Create instance with custom container
var sidebarNotify = humane.create({
  container: document.getElementById('sidebar')
});

sidebarNotify.log('Notification in sidebar');

// Create instance for specific region
var headerNotify = humane.create({
  container: document.querySelector('header'),
  baseCls: 'humane-flatty'
});

headerNotify.log('Header notification');

// Note: container must be set during instance creation
// Cannot be changed on an existing instance
```

## Advanced Usage Patterns

### Method Chaining

The `log()` method returns the Humane instance, enabling method chaining with spawned functions.

```javascript
// Create themed notifiers
var bigbox = humane.create({ baseCls: 'humane-bigbox', timeout: 1000 });
bigbox.error = bigbox.spawn({ addnCls: 'humane-bigbox-error' });
bigbox.success = bigbox.spawn({ addnCls: 'humane-bigbox-success' });

// Chain notifications (they queue and display sequentially)
bigbox.log('Starting process')
  .success('Step 1 complete')
  .success('Step 2 complete')
  .error('Step 3 failed');
```

### Multiple Instances for Different Contexts

```javascript
// Create specialized notifiers for different parts of the application
var systemNotify = humane.create({
  baseCls: 'humane-libnotify',
  timeout: 3000
});

var userNotify = humane.create({
  baseCls: 'humane-bigbox',
  timeout: 5000,
  clickToClose: true
});

// Use in different contexts
systemNotify.log('System backup started');
userNotify.log('You have a new message');

// Each instance has its own queue
systemNotify.log('Backup 10% complete');
systemNotify.log('Backup 20% complete');
userNotify.log('Another message');
```

### Dynamic Theme Switching

```javascript
// Theme switcher
function setTheme(themeName) {
  humane.baseCls = 'humane-' + themeName;
  humane.log('Theme changed to ' + themeName);
}

// Example themes: libnotify, bigbox, boldlight, jackedup, original, flatty
document.getElementById('theme-libnotify').addEventListener('click', function() {
  setTheme('libnotify');
});

document.getElementById('theme-bigbox').addEventListener('click', function() {
  setTheme('bigbox');
});
```

### Progress Notifications

```javascript
// Disable timeout for progress notifications
humane.timeout = 0;

// Show progress
humane.log('Processing: 0%');

// Update notification by removing and showing new one
var progress = 0;
var interval = setInterval(function() {
  progress += 10;
  humane.remove(function() {
    if (progress <= 100) {
      humane.log('Processing: ' + progress + '%');
    }
    if (progress >= 100) {
      clearInterval(interval);
      humane.timeout = 2500; // Restore default
      humane.remove(function() {
        humane.log('Complete!');
      });
    }
  });
}, 500);
```

## Browser Support

humane.js supports a wide range of browsers including legacy versions:

- **Internet Explorer**: 7+
- **Firefox**: 3+
- **Chrome**: 9+
- **Safari**: 3+
- **Opera**: 10+
- **iOS**: 4+
- **Android**: 2+

The library automatically detects CSS transition support and falls back to JavaScript-based opacity animation for older browsers. Touch events are supported for mobile devices.

## Themes

humane.js includes six built-in themes. To use a theme:

1. Include the theme CSS file in your HTML: `<link rel="stylesheet" href="themes/libnotify.css">`
2. Set the `baseCls` property: `humane.baseCls = 'humane-libnotify'`

**Available Themes:**

- **libnotify**: Inspired by Linux desktop notifications
- **bigbox**: Large, centered notification boxes
- **boldlight**: Bold text with light background
- **jackedup**: Energetic, attention-grabbing style
- **original**: Classic humane.js appearance
- **flatty**: Modern flat design

Custom themes can be created using Stylus with Nib and Canvas. See the `theme-src` directory in the repository for examples.

## Types

```javascript { .api }
interface Humane {
  /** Display a notification */
  log(html: string | string[], options?: HumaneOptions | Function, callback?: Function): Humane;

  /** Create a new Humane instance */
  create(options?: HumaneOptions): Humane;

  /** Create a notification function with default options */
  spawn(defaults: HumaneOptions): (html: string | string[], options?: HumaneOptions | Function, callback?: Function) => Humane;

  /** Force remove current notification */
  remove(callback?: Function): void;

  /** Configuration properties */
  timeout: number;
  waitForMove: boolean;
  clickToClose: boolean;
  timeoutAfterMove: number | boolean;
  addnCls: string;
  baseCls: string;
  container: HTMLElement;
}

interface HumaneOptions {
  /** Duration in milliseconds before notification fades (0 for no timeout) */
  timeout?: number;

  /** Wait for user interaction before removing notification */
  waitForMove?: boolean;

  /** Allow clicking/touching notification to close it */
  clickToClose?: boolean;

  /** Delay in milliseconds after user interaction before hiding */
  timeoutAfterMove?: number | boolean;

  /** Additional CSS class(es) to apply to notification */
  addnCls?: string;

  /** Base CSS class for notification element */
  baseCls?: string;

  /** DOM element to append notification to (only for create()) */
  container?: HTMLElement;
}
```
