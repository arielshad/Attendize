# Configuration Options

Bootstrap TouchSpin provides comprehensive configuration options that can be passed during initialization or set via HTML data attributes.

## Capabilities

### Passing Options

Options can be provided in two ways:

**JavaScript object during initialization:**

```javascript
$('input').TouchSpin({
  min: 0,
  max: 100,
  step: 5
});
```

**HTML data attributes:**

```html
<input type="number"
       data-bts-min="0"
       data-bts-max="100"
       data-bts-step="5">
```

### Value Constraints

Control the range and precision of values.

```javascript { .api }
/**
 * Minimum allowed value
 * @type {number}
 * @default 0
 * @data-attribute data-bts-min
 */
min: number;

/**
 * Maximum allowed value
 * @type {number}
 * @default 100
 * @data-attribute data-bts-max
 */
max: number;

/**
 * Step increment/decrement amount
 * @type {number}
 * @default 1
 * @data-attribute data-bts-step
 */
step: number;

/**
 * Number of decimal places to display
 * @type {number}
 * @default 0
 * @data-attribute data-bts-decimals
 */
decimals: number;

/**
 * How to handle values that don't align with step
 * Options: 'none', 'floor', 'round', 'ceil'
 * @type {string}
 * @default 'round'
 * @data-attribute data-bts-force-step-divisibility
 */
forcestepdivisibility: string;

/**
 * Initial value if input is empty
 * @type {string | number}
 * @default ''
 * @data-attribute data-bts-init-val
 */
initval: string | number;

/**
 * Value to use when input is invalid or empty
 * @type {string | number}
 * @default ''
 * @data-attribute data-bts-replacement-val
 */
replacementval: string | number;
```

**Usage Examples:**

```javascript
// Decimal values with precision
$('input').TouchSpin({
  min: 0,
  max: 10,
  step: 0.5,
  decimals: 2,
  initval: 5.5
});

// Force step alignment
$('input').TouchSpin({
  min: 0,
  max: 100,
  step: 10,
  forcestepdivisibility: 'round' // Values like 23 become 20
});

// Replacement value for invalid input
$('input').TouchSpin({
  min: 1,
  max: 100,
  replacementval: 50 // Invalid input becomes 50
});
```

### Button Styling

Customize the appearance and layout of increment/decrement buttons.

```javascript { .api }
/**
 * Display buttons vertically instead of horizontally
 * @type {boolean}
 * @default false
 * @data-attribute data-bts-vertical-buttons
 */
verticalbuttons: boolean;

/**
 * CSS classes for the up/increment button
 * @type {string}
 * @default 'btn btn-default'
 * @data-attribute data-bts-button-up-class
 */
buttonup_class: string;

/**
 * CSS classes for the down/decrement button
 * @type {string}
 * @default 'btn btn-default'
 * @data-attribute data-bts-button-down-class
 */
buttondown_class: string;

/**
 * Text or HTML content for the up button
 * @type {string}
 * @default '+'
 * @data-attribute data-bts-button-up-txt
 */
buttonup_txt: string;

/**
 * Text or HTML content for the down button
 * @type {string}
 * @default '-'
 * @data-attribute data-bts-button-down-txt
 */
buttondown_txt: string;

/**
 * Icon class for vertical up button
 * @type {string}
 * @default 'glyphicon glyphicon-chevron-up'
 * @data-attribute data-bts-vertical-up-class
 */
verticalupclass: string;

/**
 * Icon class for vertical down button
 * @type {string}
 * @default 'glyphicon glyphicon-chevron-down'
 * @data-attribute data-bts-vertical-down-class
 */
verticaldownclass: string;
```

**Usage Examples:**

```javascript
// Custom button styling with Bootstrap classes
$('input').TouchSpin({
  buttonup_class: 'btn btn-success',
  buttondown_class: 'btn btn-danger',
  buttonup_txt: '<i class="fa fa-plus"></i>',
  buttondown_txt: '<i class="fa fa-minus"></i>'
});

// Vertical button layout
$('input').TouchSpin({
  verticalbuttons: true,
  verticalupclass: 'glyphicon glyphicon-chevron-up',
  verticaldownclass: 'glyphicon glyphicon-chevron-down'
});
```

### Prefix and Postfix

Add text labels before or after the input field.

```javascript { .api }
/**
 * Text to display before the input
 * @type {string}
 * @default ''
 * @data-attribute data-bts-prefix
 */
prefix: string;

/**
 * Text to display after the input
 * @type {string}
 * @default ''
 * @data-attribute data-bts-postfix
 */
postfix: string;

/**
 * Additional CSS class for the prefix element
 * @type {string}
 * @default ''
 * @data-attribute data-bts-prefix-extra-class
 */
prefix_extraclass: string;

/**
 * Additional CSS class for the postfix element
 * @type {string}
 * @default ''
 * @data-attribute data-bts-postfix-extra-class
 */
postfix_extraclass: string;
```

**Usage Examples:**

```javascript
// Currency prefix
$('input').TouchSpin({
  prefix: '$',
  min: 0,
  max: 1000,
  step: 10,
  decimals: 2
});

// Unit postfix
$('input').TouchSpin({
  postfix: 'kg',
  min: 0,
  max: 500,
  step: 5,
  decimals: 1
});

// Custom styling for prefix/postfix
$('input').TouchSpin({
  prefix: '$',
  postfix: 'USD',
  prefix_extraclass: 'text-primary',
  postfix_extraclass: 'text-muted'
});
```

### Booster Feature

Configure accelerated value changes during long press.

```javascript { .api }
/**
 * Enable accelerated value changes during long press
 * When enabled, step size increases exponentially the longer you hold
 * @type {boolean}
 * @default true
 * @data-attribute data-bts-booster
 */
booster: boolean;

/**
 * Number of spins before boosting starts
 * @type {number}
 * @default 10
 * @data-attribute data-bts-boostat
 */
boostat: number;

/**
 * Maximum step size when boosting is active
 * Set to false for unlimited boosting
 * @type {number | boolean}
 * @default false
 * @data-attribute data-bts-max-boosted-step
 */
maxboostedstep: number | boolean;
```

**Usage Examples:**

```javascript
// Enable boosting with custom settings
$('input').TouchSpin({
  min: 0,
  max: 10000,
  step: 1,
  booster: true,
  boostat: 10,        // Start boosting after 10 spins
  maxboostedstep: 100 // Cap boost at 100 per step
});

// Disable boosting for precise control
$('input').TouchSpin({
  booster: false
});
```

### Interaction Features

Configure mouse and touch interaction behavior.

```javascript { .api }
/**
 * Enable mousewheel support when input is focused
 * Scroll up to increment, scroll down to decrement
 * @type {boolean}
 * @default true
 * @data-attribute data-bts-mouse-wheel
 */
mousewheel: boolean;

/**
 * Interval in milliseconds between steps when holding button
 * Lower values = faster spinning
 * @type {number}
 * @default 100
 * @data-attribute data-bts-step-interval
 */
stepinterval: number;

/**
 * Delay in milliseconds before continuous spinning starts
 * Time to wait after initial click before repeating
 * @type {number}
 * @default 500
 * @data-attribute data-bts-step-interval-delay
 */
stepintervaldelay: number;
```

**Usage Examples:**

```javascript
// Disable mousewheel
$('input').TouchSpin({
  mousewheel: false
});

// Fast spinning configuration
$('input').TouchSpin({
  stepinterval: 50,       // 50ms between steps (faster)
  stepintervaldelay: 300  // Start spinning after 300ms
});

// Slow, deliberate spinning
$('input').TouchSpin({
  stepinterval: 200,      // 200ms between steps (slower)
  stepintervaldelay: 800  // Wait longer before spinning
});
```

### Updating Options

Update settings after initialization using the `touchspin.updatesettings` event.

```javascript { .api }
/**
 * Update configuration options after initialization
 * @param {Object} newsettings - Object containing options to update
 */
$('input').trigger('touchspin.updatesettings', newsettings);
```

**Usage Example:**

```javascript
// Initialize with defaults
$('input').TouchSpin({
  min: 0,
  max: 100
});

// Later, update the range
$('input').trigger('touchspin.updatesettings', {
  min: 10,
  max: 200,
  step: 5
});
```

## Complete Configuration Example

```javascript
$('input').TouchSpin({
  // Value constraints
  min: 0,
  max: 100,
  step: 1,
  decimals: 0,
  forcestepdivisibility: 'round',
  initval: '',
  replacementval: '',

  // Button styling
  verticalbuttons: false,
  buttonup_class: 'btn btn-default',
  buttondown_class: 'btn btn-default',
  buttonup_txt: '+',
  buttondown_txt: '-',
  verticalupclass: 'glyphicon glyphicon-chevron-up',
  verticaldownclass: 'glyphicon glyphicon-chevron-down',

  // Prefix and postfix
  prefix: '',
  postfix: '',
  prefix_extraclass: '',
  postfix_extraclass: '',

  // Booster feature
  booster: true,
  boostat: 10,
  maxboostedstep: false,

  // Interaction
  mousewheel: true,
  stepinterval: 100,
  stepintervaldelay: 500
});
```
