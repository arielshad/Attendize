# jQuery MiniColors

jQuery MiniColors is a lightweight, highly customizable color picker plugin built on jQuery. It transforms text input elements into interactive color pickers with multiple control modes, support for various color formats (hex, RGB, RGBA), opacity controls, predefined color swatches, and customizable themes.

## Package Information

- **Package Name**: jquery-minicolors
- **Package Type**: npm
- **Language**: JavaScript
- **Installation**: `npm install jquery-minicolors`
- **Dependencies**: jQuery >= 1.7.x

## Core Imports

```javascript
// Browser globals (when included via script tag)
// The plugin extends jQuery automatically

// CommonJS
const $ = require('jquery');
require('jquery-minicolors');

// AMD
define(['jquery', 'jquery-minicolors'], function($) {
  // Use $.fn.minicolors
});
```

CSS must also be included:

```html
<link rel="stylesheet" href="jquery.minicolors.css">
```

## Basic Usage

```javascript
// Initialize a color picker on an input element
$('#colorInput').minicolors();

// Initialize with custom options
$('#colorInput').minicolors({
  control: 'hue',
  format: 'hex',
  opacity: false,
  theme: 'default',
  change: function(value, opacity) {
    console.log('Color changed to:', value);
  }
});

// Set a color value
$('#colorInput').minicolors('value', '#ff6600');

// Get the current color value
const color = $('#colorInput').minicolors('value');

// Get RGB object representation
const rgb = $('#colorInput').minicolors('rgbObject');
// Returns: { r: 255, g: 102, b: 0, a: 1 }
```

## Capabilities

### Initialization

Initialize the color picker plugin on input elements.

```javascript { .api }
/**
 * Initialize minicolors on selected input elements
 * @param {Object} settings - Configuration options (optional)
 * @returns {jQuery} jQuery object for chaining
 */
$(selector).minicolors(settings);

// Equivalent explicit form:
$(selector).minicolors('create', settings);
```

**Usage Example:**

```javascript
// Basic initialization
$('input[type="text"]').minicolors();

// With configuration
$('#myColorInput').minicolors({
  control: 'wheel',
  format: 'rgb',
  opacity: true,
  position: 'top left',
  theme: 'bootstrap',
  swatches: ['#fff', '#000', '#f00', '#0f0', '#00f'],
  change: function(value, opacity) {
    $('#preview').css('background-color', value);
  }
});
```

### Color Value Management

Get or set the color value of the picker.

```javascript { .api }
/**
 * Get the current color value
 * @returns {string} Current color value
 */
$(selector).minicolors('value');

/**
 * Set the color value
 * @param {string|Object} value - Color string (hex/rgb/rgba) or object with color and opacity
 * @returns {jQuery} jQuery object for chaining
 */
$(selector).minicolors('value', value);
```

**Usage Examples:**

```javascript
// Get current value
const currentColor = $('#colorInput').minicolors('value');

// Set hex color
$('#colorInput').minicolors('value', '#ff6600');

// Set with opacity
$('#colorInput').minicolors('value', {
  color: '#ff6600',
  opacity: 0.5
});

// Set RGB color
$('#colorInput').minicolors('value', 'rgb(255, 102, 0)');
```

### Opacity Management

Get or set the opacity level of the color.

```javascript { .api }
/**
 * Get the current opacity value
 * @returns {string} Opacity value (0-1)
 */
$(selector).minicolors('opacity');

/**
 * Set the opacity value
 * @param {number} value - Opacity value between 0 and 1
 * @returns {jQuery} jQuery object for chaining
 */
$(selector).minicolors('opacity', value);
```

**Usage Examples:**

```javascript
// Get opacity
const opacity = $('#colorInput').minicolors('opacity');

// Set opacity
$('#colorInput').minicolors('opacity', 0.75);
```

### Color Format Conversion

Convert the current color to different formats.

```javascript { .api }
/**
 * Get RGB object representation
 * @returns {Object} Object with r, g, b, and optionally a properties
 */
$(selector).minicolors('rgbObject');

/**
 * Get RGB string representation
 * @returns {string} RGB string like "rgb(255, 102, 0)"
 */
$(selector).minicolors('rgbString');

/**
 * Get RGBA string representation
 * @returns {string} RGBA string like "rgba(255, 102, 0, 0.5)"
 */
$(selector).minicolors('rgbaString');
```

**Usage Examples:**

```javascript
// Get RGB object
const rgb = $('#colorInput').minicolors('rgbObject');
console.log(rgb); // { r: 255, g: 102, b: 0, a: 1 }

// Get RGB string
const rgbString = $('#colorInput').minicolors('rgbString');
console.log(rgbString); // "rgb(255, 102, 0)"

// Get RGBA string
const rgbaString = $('#colorInput').minicolors('rgbaString');
console.log(rgbaString); // "rgba(255, 102, 0, 1)"
```

### Picker Display Control

Show or hide the color picker dropdown.

```javascript { .api }
/**
 * Show the color picker dropdown
 * @returns {jQuery} jQuery object for chaining
 */
$(selector).minicolors('show');

/**
 * Hide the color picker dropdown
 * @returns {jQuery} jQuery object for chaining
 */
$(selector).minicolors('hide');
```

**Usage Examples:**

```javascript
// Show picker programmatically
$('#colorInput').minicolors('show');

// Hide picker programmatically
$('#colorInput').minicolors('hide');
```

### Settings Management

Get or update the color picker settings.

```javascript { .api }
/**
 * Get current settings
 * @returns {Object} Current settings object
 */
$(selector).minicolors('settings');

/**
 * Update settings (destroys and re-initializes the picker)
 * @param {Object} settings - New settings to apply
 * @returns {jQuery} jQuery object for chaining
 */
$(selector).minicolors('settings', settings);
```

**Usage Examples:**

```javascript
// Get current settings
const currentSettings = $('#colorInput').minicolors('settings');

// Update settings
$('#colorInput').minicolors('settings', {
  control: 'wheel',
  opacity: true,
  theme: 'bootstrap'
});
```

### Destruction

Remove the color picker and restore the input to its original state.

```javascript { .api }
/**
 * Destroy the color picker
 * @returns {jQuery} jQuery object for chaining
 */
$(selector).minicolors('destroy');
```

**Usage Example:**

```javascript
// Remove color picker
$('#colorInput').minicolors('destroy');
```

## Configuration Options

All configuration options can be set during initialization or via the `settings` method.

```javascript { .api }
interface MinicolorsSettings {
  /** Animation speed in milliseconds (default: 50) */
  animationSpeed?: number;

  /** jQuery easing function for animations (default: 'swing') */
  animationEasing?: string;

  /** Callback function when color changes */
  change?: (value: string, opacity: number) => void;

  /** Delay in milliseconds before firing change event (default: 0) */
  changeDelay?: number;

  /** Control type: 'hue', 'saturation', 'brightness', 'wheel' (default: 'hue') */
  control?: 'hue' | 'saturation' | 'brightness' | 'wheel';

  /** Use data URIs for images (default: true, set false for IE7/IE8) */
  dataUris?: boolean;

  /** Default color value when input is cleared (default: '') */
  defaultValue?: string;

  /** Color format: 'hex' or 'rgb' (default: 'hex') */
  format?: 'hex' | 'rgb';

  /** Callback function when picker is hidden */
  hide?: () => void;

  /** Hide animation speed in milliseconds (default: 100) */
  hideSpeed?: number;

  /** Display picker inline instead of dropdown (default: false) */
  inline?: boolean;

  /** Comma-separated CSS keywords to accept (default: '') */
  keywords?: string;

  /** Letter case: 'lowercase' or 'uppercase' (default: 'lowercase') */
  letterCase?: 'lowercase' | 'uppercase';

  /** Enable opacity slider (default: false) */
  opacity?: boolean;

  /** Picker position: 'bottom left', 'bottom right', 'top left', 'top right' (default: 'bottom left') */
  position?: 'bottom left' | 'bottom right' | 'top left' | 'top right';

  /** Callback function when picker is shown */
  show?: () => void;

  /** Show animation speed in milliseconds (default: 100) */
  showSpeed?: number;

  /** Theme name for custom styling (default: 'default') */
  theme?: string;

  /** Array of predefined color swatches, max 7 (default: []) */
  swatches?: string[];
}
```

### Global Default Settings

Modify default settings for all future instances:

```javascript { .api }
// Access global defaults
$.minicolors.defaults;

// Modify a single default
$.minicolors.defaults.changeDelay = 200;

// Modify multiple defaults
$.minicolors.defaults = $.extend($.minicolors.defaults, {
  changeDelay: 200,
  letterCase: 'uppercase',
  theme: 'bootstrap'
});
```

**Note:** Changing default settings does not affect already initialized color pickers.

## Event Callbacks

### change

Fires when the color value changes. Can fire frequently while dragging.

```javascript { .api }
/**
 * Change event callback
 * @param {string} value - New color value
 * @param {number} opacity - Opacity value (0-1)
 * @this {HTMLInputElement} Original input element
 */
change: function(value, opacity) {
  // this refers to the input element
}
```

**Usage Example:**

```javascript
$('#colorInput').minicolors({
  change: function(value, opacity) {
    console.log('Color:', value, 'Opacity:', opacity);
    // Update UI based on color change
    $('#preview').css({
      'background-color': value,
      'opacity': opacity
    });
  },
  changeDelay: 200 // Throttle callback to every 200ms
});
```

### show

Fires when the color picker dropdown is shown.

```javascript { .api }
/**
 * Show event callback
 * @this {HTMLInputElement} Original input element
 */
show: function() {
  // this refers to the input element
}
```

**Usage Example:**

```javascript
$('#colorInput').minicolors({
  show: function() {
    console.log('Picker opened');
    $(this).addClass('picker-active');
  }
});
```

### hide

Fires when the color picker dropdown is hidden.

```javascript { .api }
/**
 * Hide event callback
 * @this {HTMLInputElement} Original input element
 */
hide: function() {
  // this refers to the input element
}
```

**Usage Example:**

```javascript
$('#colorInput').minicolors({
  hide: function() {
    console.log('Picker closed');
    $(this).removeClass('picker-active');
  }
});
```

## Control Modes

The plugin offers four different control modes for color selection:

### hue (default)

Vertical hue slider with saturation/brightness grid. Best for general purpose color selection.

```javascript
$('#colorInput').minicolors({ control: 'hue' });
```

### saturation

Vertical saturation slider with hue/brightness grid. Useful when saturation is the primary adjustment.

```javascript
$('#colorInput').minicolors({ control: 'saturation' });
```

### brightness

Vertical brightness slider with hue/saturation grid. Useful when brightness is the primary adjustment.

```javascript
$('#colorInput').minicolors({ control: 'brightness' });
```

### wheel

Circular hue/saturation wheel with brightness slider. Provides a more traditional color wheel interface.

```javascript
$('#colorInput').minicolors({ control: 'wheel' });
```

## Color Formats

### Hex Format

```javascript
$('#colorInput').minicolors({
  format: 'hex',
  letterCase: 'lowercase' // or 'uppercase'
});

// Input/output: "#ff6600" or "#FF6600"
```

### RGB/RGBA Format

```javascript
$('#colorInput').minicolors({
  format: 'rgb',
  opacity: false // Use true for RGBA
});

// RGB output: "rgb(255, 102, 0)"
// RGBA output: "rgba(255, 102, 0, 0.5)"
```

### CSS Keywords

```javascript
$('#colorInput').minicolors({
  keywords: 'transparent, initial, inherit'
});

// Accepts keywords like "transparent", "initial", "inherit"
```

## Advanced Features

### Color Swatches

Provide predefined color options (maximum 7):

```javascript
$('#colorInput').minicolors({
  swatches: [
    '#fff',
    '#000',
    '#f00',
    '#0f0',
    '#00f',
    '#ff0',
    'rgba(0, 0, 255, 0.5)'
  ]
});
```

Swatches appear below the color grid and can be clicked to quickly select predefined colors.

### Inline Mode

Display the picker inline instead of as a dropdown:

```javascript
$('#colorInput').minicolors({
  inline: true
});
```

The picker will be permanently visible next to the input field.

### Custom Themes

Apply custom themes for styling:

```javascript
$('#colorInput').minicolors({
  theme: 'bootstrap'
});
```

Custom themes are applied via CSS using the class `.minicolors-theme-{themeName}`.

### Opacity Control

Enable opacity/alpha channel control:

```javascript
$('#colorInput').minicolors({
  opacity: true
});
```

Set preset opacity via HTML attribute:

```html
<input type="text" data-opacity="0.5" />
```

### Animation Control

Customize animation behavior:

```javascript
$('#colorInput').minicolors({
  animationSpeed: 100,
  animationEasing: 'linear',
  showSpeed: 200,
  hideSpeed: 200
});
```

Set `animationSpeed: 0` to disable animations.

### Position Control

Control where the dropdown appears:

```javascript
$('#colorInput').minicolors({
  position: 'top right'
});
```

Options: `'bottom left'`, `'bottom right'`, `'top left'`, `'top right'`

## Types

```javascript { .api }
// RGB/RGBA Object
interface RGBObject {
  r: number; // Red (0-255)
  g: number; // Green (0-255)
  b: number; // Blue (0-255)
  a?: number; // Alpha/opacity (0-1), optional
}

// Settings object (see Configuration Options section above for full interface)
interface MinicolorsSettings {
  animationSpeed?: number;
  animationEasing?: string;
  change?: (value: string, opacity: number) => void;
  changeDelay?: number;
  control?: 'hue' | 'saturation' | 'brightness' | 'wheel';
  dataUris?: boolean;
  defaultValue?: string;
  format?: 'hex' | 'rgb';
  hide?: () => void;
  hideSpeed?: number;
  inline?: boolean;
  keywords?: string;
  letterCase?: 'lowercase' | 'uppercase';
  opacity?: boolean;
  position?: 'bottom left' | 'bottom right' | 'top left' | 'top right';
  show?: () => void;
  showSpeed?: number;
  theme?: string;
  swatches?: string[];
}

// Value setter can accept string or object
type MinicolorsValue = string | {
  color: string;
  opacity?: number;
};
```

## Native jQuery Events

In addition to the callback options, the plugin also triggers native jQuery events on the input element:

- `change` - Triggered when the color value changes
- `input` - Triggered when the color value changes

```javascript
$('#colorInput').on('change', function() {
  console.log('Color changed via native event');
});
```

## HTML Data Attributes

The plugin reads the following HTML data attributes:

- `data-opacity` - Set preset opacity value (0-1)

```html
<input type="text" id="colorInput" value="#ff6600" data-opacity="0.75" />
```

## Browser Compatibility

- Modern browsers: Full support
- IE7/IE8: Set `dataUris: false` to use external images instead of data URIs

```javascript
$('#colorInput').minicolors({
  dataUris: false // Required for IE7/IE8
});
```
