# jQuery.payment

A general purpose library for building credit card forms, validating inputs and formatting numbers. jQuery.payment provides both DOM manipulation methods for real-time input formatting and static validation functions for programmatic use.

## Package Information

- **Package Name**: jquery.payment
- **Package Type**: bower
- **Language**: JavaScript
- **Installation**: `bower install jquery.payment`
- **Alternative**: `npm install jquery.payment` (also available on npm)

## Core Imports

jQuery:

```javascript
<script src="jquery.js"></script>
<script src="jquery.payment.js"></script>
```

For module systems:

```javascript
var $ = require('jquery');
require('jquery.payment');
```

The library extends jQuery's `$` object with `$.payment` namespace and `$.fn.payment()` plugin method.

## Basic Usage

```javascript
// Apply formatting to input fields
$('input.cc-number').payment('formatCardNumber');
$('input.cc-expiry').payment('formatCardExpiry');
$('input.cc-cvc').payment('formatCardCVC');

// Validate inputs
var cardNumber = $('input.cc-number').val();
var isValid = $.payment.validateCardNumber(cardNumber);

if (!isValid) {
  alert('Invalid card number!');
}

// Detect card type
var cardType = $.payment.cardType(cardNumber);
console.log(cardType); // 'visa', 'mastercard', etc.
```

## Architecture

jQuery.payment provides two main interfaces:

- **jQuery Plugin Methods** (`$.fn.payment()`): DOM manipulation methods that attach event handlers to format and restrict input in real-time
- **Static Utility Functions** (`$.payment.*`): Standalone functions for validation, formatting, and card type detection that can be called programmatically

The library automatically handles cursor positioning during formatting, supports full-width numeral conversion (for Japanese input), and adds CSS classes to inputs for styling based on detected card types.

## Supported Card Types

The library recognizes and validates the following card types:

- Visa (4...)
- MasterCard (51-55, 22-27)
- American Express (34, 37)
- Diners Club (30, 36, 38, 39)
- Discover (60, 64, 65, 622)
- UnionPay (62, 88)
- JCB (35)
- Visa Electron (4026, 417500, 4405, 4508, 4844, 4913, 4917)
- Maestro (5018, 502, 503, 506, 56, 58, 639, 6220, 67)
- Forbrugsforeningen (600)
- Dankort (5019)
- Elo (4011, 4312, 4389, 4514, 4573, 4576, 5041, 5066, 5067, 509, 6277, 6362, 6363, 650, 6516, 6550)

Additional card types can be added by extending the `$.payment.cards` array.

## Capabilities

### Input Formatting

Real-time formatting for credit card input fields using jQuery plugin methods. Automatically adds spacing, restricts input to valid characters, enforces length limits, and detects card types.

```javascript { .api }
$('input.cc-number').payment('formatCardNumber');
$('input.cc-expiry').payment('formatCardExpiry');
$('input.cc-cvc').payment('formatCardCVC');
$('input.numeric').payment('restrictNumeric');
```

[Input Formatting](./formatting.md)

### Card Validation

Client-side validation functions for card numbers, expiry dates, and CVC codes. Validates using Luhn algorithm, checks expiry dates are in the future, and validates CVC length based on card type.

```javascript { .api }
function validateCardNumber(number: string): boolean;
function validateCardExpiry(month: string | number | object, year?: string | number): boolean;
function validateCardCVC(cvc: string, type?: string): boolean;
```

[Card Validation](./validation.md)

### Utility Functions

Card type detection, expiry parsing, and programmatic formatting functions. Includes extensible card type definitions.

```javascript { .api }
function cardType(number: string): string | null;
function cardExpiryVal(expiry: string): { month: number, year: number };
function formatCardNumber(number: string): string;
function formatExpiry(expiry: string): string;

// Card type definitions (extensible array)
var cards: Array<object>;
```

[Utility Functions](./utilities.md)

## Events

### payment.cardType

Custom jQuery event fired when the detected card type changes on an input with `formatCardNumber` applied.

```javascript { .api }
$('input.cc-number')
  .payment('formatCardNumber')
  .on('payment.cardType', function(event, cardType) {
    // cardType is one of: 'visa', 'mastercard', 'amex', etc., or 'unknown'
    console.log('Card type:', cardType);
  });
```

The event handler receives the card type as the second argument. The card type will be `'unknown'` if the card cannot be identified.

Additionally, the library automatically adds a CSS class matching the card type to the input element (e.g., `class="visa"` or `class="unknown"`), and adds the class `identified` when a card type is successfully detected.

## Dependencies

- jQuery >= 1.5 (or Zepto >= 1.1)
