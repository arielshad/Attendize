# Input Formatting

Real-time input formatting methods that attach event handlers to DOM elements. These jQuery plugin methods automatically format user input as they type, restrict invalid characters, enforce length limits, and provide visual feedback through CSS classes.

## Capabilities

### Format Card Number

Applies real-time formatting to credit card number inputs with automatic spacing, numeric restriction, length limiting, and card type detection.

```javascript { .api }
/**
 * Format card number input with automatic spacing and type detection
 *
 * Behavior:
 * - Adds space every 4 digits (or 4-6-5 for American Express)
 * - Restricts input to numbers only
 * - Limits length to card-specific maximum (13-19 digits depending on type)
 * - Adds CSS class for detected card type ('visa', 'mastercard', 'amex', etc.)
 * - Adds 'identified' class when card type is detected
 * - Fires 'payment.cardType' event on type change
 * - Handles full-width numerals (converts to half-width)
 * - Maintains cursor position during formatting
 *
 * @returns jQuery object for chaining
 */
$('selector').payment('formatCardNumber');
```

**Usage Example:**

```javascript
// Apply formatting to card number input
$('input.cc-number').payment('formatCardNumber');

// Listen for card type changes
$('input.cc-number').on('payment.cardType', function(event, type) {
  console.log('Card type detected:', type);
  // Update UI based on card type
  if (type === 'amex') {
    $('.cvc-hint').text('4 digits on front');
  } else {
    $('.cvc-hint').text('3 digits on back');
  }
});
```

**CSS Classes Added:**

- Card type classes: `visa`, `mastercard`, `amex`, `dinersclub`, `discover`, `unionpay`, `jcb`, `visaelectron`, `maestro`, `forbrugsforeningen`, `dankort`, `elo`
- `unknown` - when card type cannot be determined
- `identified` - when card type is successfully detected (removed when type is 'unknown')

### Format Card Expiry

Applies real-time formatting to card expiry inputs with automatic slash insertion and MM / YYYY formatting.

```javascript { .api }
/**
 * Format card expiry input with automatic slash insertion
 *
 * Behavior:
 * - Automatically adds ' / ' separator between month and year
 * - Pads single-digit months with leading zero when appropriate
 * - Restricts input to numbers only
 * - Limits to 6 numeric digits (MM / YYYY)
 * - Handles shortcuts: typing '/' or space after single digit prepends '0'
 * - Handles full-width numerals (converts to half-width)
 * - Maintains cursor position during formatting
 *
 * @returns jQuery object for chaining
 */
$('selector').payment('formatCardExpiry');
```

**Usage Example:**

```javascript
// Apply formatting to expiry input
$('input.cc-expiry').payment('formatCardExpiry');

// Get parsed expiry value
var expiry = $('input.cc-expiry').payment('cardExpiryVal');
console.log(expiry.month, expiry.year); // { month: 3, year: 2025 }
```

**Formatting Behavior:**

| User Input | Formatted Result | Description |
|------------|------------------|-------------|
| `4` | `04 / ` | Single digit 4-9 prepends 0 and adds separator |
| `12` | `12 / ` | Valid month adds separator |
| `14` | `01 / 4` | Invalid month splits into `01 / 4` |
| `1/` | `01 / ` | Slash after single digit prepends 0 |
| `03 / 2025` | `03 / 2025` | Complete date formats correctly |

### Format Card CVC

Applies real-time formatting to CVC/CVV inputs with numeric restriction and length limiting.

```javascript { .api }
/**
 * Format CVC/CVV input with numeric restriction and length limit
 *
 * Behavior:
 * - Restricts input to numbers only
 * - Limits to 4 digits maximum
 * - Handles full-width numerals (converts to half-width)
 * - No automatic formatting or spacing applied
 *
 * @returns jQuery object for chaining
 */
$('selector').payment('formatCardCVC');
```

**Usage Example:**

```javascript
// Apply formatting to CVC input
$('input.cc-cvc').payment('formatCardCVC');
```

### Restrict Numeric

General-purpose numeric input restriction for any input field.

```javascript { .api }
/**
 * Restrict input to numeric characters only
 *
 * Behavior:
 * - Allows numbers 0-9 only
 * - Blocks all non-numeric characters
 * - Allows control keys (backspace, delete, arrows, etc.)
 * - Handles full-width numerals (converts to half-width)
 * - No length restriction or formatting applied
 *
 * @returns jQuery object for chaining
 */
$('selector').payment('restrictNumeric');
```

**Usage Example:**

```javascript
// Apply to any input that should only accept numbers
$('[data-numeric]').payment('restrictNumeric');
$('input.zip-code').payment('restrictNumeric');
```

### Get Card Expiry Value

jQuery plugin method to parse expiry value from an input element.

```javascript { .api }
/**
 * Parse expiry value from input element
 *
 * @returns Object with month and year properties
 */
$('selector').payment('cardExpiryVal'): { month: number, year: number };
```

**Usage Example:**

```javascript
// Get parsed expiry from input
var expiry = $('input.cc-expiry').payment('cardExpiryVal');

if (expiry.month && expiry.year) {
  console.log('Month:', expiry.month); // 1-12
  console.log('Year:', expiry.year);   // 4-digit year
}
```

**Note:** This method parses the current input value but does not validate it. Use `$.payment.validateCardExpiry()` for validation.

## Implementation Notes

### Event Handlers

All formatting methods attach multiple event handlers to the input element:

- `keypress` - Intercepts key presses to format in real-time
- `keydown` - Handles backspace/delete for smart formatting removal
- `keyup` - Updates card type detection (card number only)
- `paste` - Reformats pasted content
- `change` - Reformats on change events
- `input` - Reformats on any input change

### Cursor Position

The library intelligently maintains cursor position during formatting operations:

- Cursor stays in the correct position when spaces are added
- Automatic cursor advancement when space is inserted
- Preserves selection when reformatting pasted content

### Full-Width Numeral Support

All formatting methods automatically convert full-width numerals (Japanese input: ０１２３４５６７８９) to half-width numerals (0123456789) for proper processing.

### Chaining

All plugin methods return the jQuery object for chaining:

```javascript
$('input.cc-number')
  .payment('formatCardNumber')
  .on('payment.cardType', handler)
  .addClass('my-custom-class');
```

## Recommended HTML

For best user experience, use appropriate HTML attributes:

```html
<!-- Card number -->
<input type="tel" class="cc-number" autocomplete="cc-number">

<!-- Expiry -->
<input type="tel" class="cc-expiry" autocomplete="cc-exp">

<!-- CVC (disable autocomplete for security) -->
<input type="tel" class="cc-cvc" autocomplete="off">
```

Using `type="tel"` displays the numeric keyboard on mobile devices. The `autocomplete` attributes follow the HTML Autofill spec and are respected by modern browsers.
