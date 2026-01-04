# Bootstrap TouchSpin

Bootstrap TouchSpin is a jQuery plugin that provides a mobile and touch-friendly numeric input spinner component for Bootstrap 3 applications. The library enhances standard HTML number inputs with increment/decrement buttons, supporting both horizontal and vertical button layouts with customizable styling.

## Package Information

- **Package Name**: bootstrap-touchspin
- **Package Type**: bower
- **Language**: JavaScript
- **Installation**: `bower install bootstrap-touchspin`
- **Dependencies**: jQuery (>=1.9.0), Bootstrap (>=3.0.0)
- **Files**:
  - JavaScript: `dist/jquery.bootstrap-touchspin.js`
  - CSS: `dist/jquery.bootstrap-touchspin.css`

## Core Imports

```html
<!-- Include jQuery and Bootstrap first -->
<script src="jquery.min.js"></script>
<script src="bootstrap.min.js"></script>
<link href="bootstrap.min.css" rel="stylesheet">

<!-- Include Bootstrap TouchSpin -->
<link href="jquery.bootstrap-touchspin.css" rel="stylesheet">
<script src="jquery.bootstrap-touchspin.js"></script>
```

## Basic Usage

```javascript
// Initialize with default settings
$("input[type='number']").TouchSpin();

// Initialize with custom options
$("input[type='number']").TouchSpin({
  min: 0,
  max: 100,
  step: 1,
  decimals: 0,
  prefix: '$',
  postfix: 'items'
});

// Update settings after initialization
$("input").trigger('touchspin.updatesettings', { min: 10, max: 200 });

// Get current value
var value = $("input").val();

// Destroy the plugin (removes all TouchSpin functionality)
$("input").TouchSpin('destroy');
```

## HTML Structure

Basic input element:

```html
<input type="number" value="50" name="demo1">
```

With data attributes for configuration:

```html
<input type="number"
       value="50"
       name="demo2"
       data-bts-min="0"
       data-bts-max="100"
       data-bts-step="5"
       data-bts-decimals="2"
       data-bts-prefix="$"
       data-bts-postfix="USD">
```

## Architecture

Bootstrap TouchSpin is built around the following components:

- **jQuery Plugin Pattern**: Extends jQuery with the `TouchSpin()` method
- **Event System**: Provides triggerable and observable custom events for programmatic control and monitoring
- **Bootstrap Integration**: Seamlessly integrates with Bootstrap 3's input-group system and button styles
- **Touch/Mouse Support**: Handles both desktop (mouse, keyboard, mousewheel) and mobile (touch) interactions
- **Value Validation**: Automatically constrains values to min/max bounds and formats according to decimal settings
- **Booster Feature**: Exponentially increases step size during long presses for faster value changes

## Capabilities

### Initialization and Configuration

Initialize TouchSpin on input elements and configure behavior through comprehensive options including value constraints, step intervals, button styling, and prefix/postfix labels.

```javascript { .api }
/**
 * Initialize TouchSpin on selected input elements
 * @param {Object|String} options - Configuration object or 'destroy' command
 * @returns {jQuery} jQuery object for chaining
 */
$.fn.TouchSpin(options);
```

[Configuration Options](./configuration.md)

### Event-Based Control

Programmatically control the spinner and respond to value changes through a comprehensive event system with both triggerable commands and observable notifications.

```javascript { .api }
// Triggerable events (commands)
$('input').trigger('touchspin.uponce');           // Increment value once
$('input').trigger('touchspin.downonce');         // Decrement value once
$('input').trigger('touchspin.startupspin');      // Start continuous upward spinning
$('input').trigger('touchspin.startdownspin');    // Start continuous downward spinning
$('input').trigger('touchspin.stopspin');         // Stop any active spinning
$('input').trigger('touchspin.updatesettings', { min: 5, max: 50 }); // Update settings

// Observable events (notifications) - use .on() to listen
$('input').on('touchspin.on.min', function() { /* ... */ });        // Minimum reached
$('input').on('touchspin.on.max', function() { /* ... */ });        // Maximum reached
$('input').on('touchspin.on.startspin', function() { /* ... */ });  // Spinning started
$('input').on('touchspin.on.stopspin', function() { /* ... */ });   // Spinning stopped
$('input').on('change', function() { /* ... */ });                  // Value changed
```

[Events](./events.md)

### Value Constraints and Validation

Configure minimum and maximum value bounds, step increments, decimal precision, and automatic value validation to ensure input stays within valid ranges.

```javascript { .api }
interface ValueConstraints {
  /** Minimum allowed value (default: 0) */
  min: number;
  /** Maximum allowed value (default: 100) */
  max: number;
  /** Step increment/decrement amount (default: 1) */
  step: number;
  /** Number of decimal places (default: 0) */
  decimals: number;
  /** How to handle non-step-aligned values: 'none', 'floor', 'round', 'ceil' (default: 'round') */
  forcestepdivisibility: string;
  /** Initial value if input is empty (default: '') */
  initval: string | number;
  /** Replacement value for invalid input (default: '') */
  replacementval: string | number;
}
```

[Configuration Options](./configuration.md)

### Button Styling and Layout

Customize button appearance with Bootstrap classes, configure horizontal or vertical button layouts, and add prefix/postfix labels for units or currency symbols.

```javascript { .api }
interface ButtonStyling {
  /** Enable vertical button layout (default: false) */
  verticalbuttons: boolean;
  /** CSS classes for up button (default: 'btn btn-default') */
  buttonup_class: string;
  /** CSS classes for down button (default: 'btn btn-default') */
  buttondown_class: string;
  /** Text/HTML for up button (default: '+') */
  buttonup_txt: string;
  /** Text/HTML for down button (default: '-') */
  buttondown_txt: string;
  /** Icon class for vertical up button (default: 'glyphicon glyphicon-chevron-up') */
  verticalupclass: string;
  /** Icon class for vertical down button (default: 'glyphicon glyphicon-chevron-down') */
  verticaldownclass: string;
  /** Text to display before input (default: '') */
  prefix: string;
  /** Text to display after input (default: '') */
  postfix: string;
  /** Additional CSS class for prefix element (default: '') */
  prefix_extraclass: string;
  /** Additional CSS class for postfix element (default: '') */
  postfix_extraclass: string;
}
```

[Configuration Options](./configuration.md)

### Advanced Features

Enable booster functionality for accelerated value changes during long presses, mousewheel support for desktop interactions, and fine-tune timing intervals for spinning behavior.

```javascript { .api }
interface AdvancedFeatures {
  /** Enable accelerated value changes during long press (default: true) */
  booster: boolean;
  /** Number of spins before boosting starts (default: 10) */
  boostat: number;
  /** Maximum step size when boosting (default: false) */
  maxboostedstep: number | boolean;
  /** Enable mousewheel support when input is focused (default: true) */
  mousewheel: boolean;
  /** Interval in milliseconds between steps when holding button (default: 100) */
  stepinterval: number;
  /** Delay in milliseconds before continuous spinning starts (default: 500) */
  stepintervaldelay: number;
}
```

[Configuration Options](./configuration.md)

### Lifecycle Management

Destroy TouchSpin instances to clean up event listeners and remove the generated HTML wrapper, returning the input element to its original state.

```javascript { .api }
/**
 * Destroy TouchSpin instance on input element
 * Removes all event listeners and generated HTML wrapper
 * @param {'destroy'} command - The destroy command string
 * @returns {jQuery} jQuery object for chaining
 */
$.fn.TouchSpin('destroy');
```

## CSS Classes

The plugin adds the following CSS classes to the generated elements:

```css
/* Main wrapper */
.bootstrap-touchspin { }

/* Buttons */
.bootstrap-touchspin-up { }
.bootstrap-touchspin-down { }

/* Prefix and postfix */
.bootstrap-touchspin-prefix { }
.bootstrap-touchspin-postfix { }

/* Vertical button container */
.input-group-btn-vertical { }
```

## Browser Support

- **Desktop**: Full mouse, keyboard (arrow keys), and mousewheel support
- **Mobile/Touch**: Touch-optimized event handling for touchstart, touchend, touchmove
- **Keyboard**: Arrow up/down keys on input, Space/Enter keys on buttons
- **Accessibility**: Maintains standard form input behavior for compatibility
