# Utility Functions

Card type detection, expiry parsing, and programmatic formatting functions. Includes the extensible card type definitions array.

## Capabilities

### Card Type Detection

Detects the card type from a card number or partial card number.

```javascript { .api }
/**
 * Detect card type from card number
 *
 * Analyzes the card number prefix to determine the card type.
 * Works with partial numbers (e.g., first 4 digits).
 *
 * @param number - Card number or partial card number
 * @returns Card type string or null if not recognized
 *
 * Possible return values:
 * - 'visa'
 * - 'mastercard'
 * - 'amex'
 * - 'dinersclub'
 * - 'discover'
 * - 'unionpay'
 * - 'jcb'
 * - 'visaelectron'
 * - 'maestro'
 * - 'forbrugsforeningen'
 * - 'dankort'
 * - 'elo'
 * - null (if card type cannot be determined)
 */
$.payment.cardType(number: string): string | null;
```

**Usage Examples:**

```javascript
// Detect card type
$.payment.cardType('4242424242424242');    // 'visa'
$.payment.cardType('5555555555554444');    // 'mastercard'
$.payment.cardType('378282246310005');     // 'amex'
$.payment.cardType('6011111111111117');    // 'discover'
$.payment.cardType('3530111333300000');    // 'jcb'

// Works with partial numbers
$.payment.cardType('4242');                // 'visa'
$.payment.cardType('34');                  // 'amex'
$.payment.cardType('6011');                // 'discover'

// Unknown cards return null
$.payment.cardType('9999999999999999');    // null
$.payment.cardType('');                    // null
$.payment.cardType('aoeu');                // null
```

**Card Type Identification:**

Card types are identified by matching the number prefix against patterns defined in `$.payment.cards`. The array is traversed in order, so more specific patterns should appear first.

### Parse Card Expiry

Parses a card expiry string into month and year components.

```javascript { .api }
/**
 * Parse expiry string into month and year
 *
 * Parses various expiry formats and returns an object with numeric
 * month and year. Converts 2-digit years to 4-digit years.
 *
 * @param expiry - Expiry string (e.g., "MM/YY", "MM / YYYY", "MM-YY")
 * @returns Object with month (number) and year (number) properties
 *
 * Note: This function does NOT validate the date. Use validateCardExpiry()
 * for validation.
 */
$.payment.cardExpiryVal(expiry: string): { month: number, year: number };
```

**Usage Examples:**

```javascript
// Various formats supported
$.payment.cardExpiryVal('03 / 2025');
// { month: 3, year: 2025 }

$.payment.cardExpiryVal('05/04');
// { month: 5, year: 2004 } - assumes 20xx for years < 70

$.payment.cardExpiryVal('12-25');
// { month: 12, year: 2025 }

$.payment.cardExpiryVal('1/2026');
// { month: 1, year: 2026 }

// Invalid input returns NaN values
$.payment.cardExpiryVal('invalid');
// { month: NaN, year: NaN }
```

**Year Conversion:**

- 2-digit years are converted to 4-digit years
- Current century prefix is used (e.g., 25 → 2025)
- Check for NaN if parsing fails

**jQuery Plugin Variant:**

```javascript { .api }
/**
 * Parse expiry value from input element
 *
 * Convenience method that gets the value from an input element
 * and parses it using $.payment.cardExpiryVal()
 *
 * @returns Object with month and year properties
 */
$('selector').payment('cardExpiryVal'): { month: number, year: number };
```

**Usage Example:**

```javascript
// Get and parse expiry from input
var expiry = $('input.cc-expiry').payment('cardExpiryVal');

if (!isNaN(expiry.month) && !isNaN(expiry.year)) {
  console.log('Month:', expiry.month); // 1-12
  console.log('Year:', expiry.year);   // 4-digit year
}
```

### Format Card Number

Programmatically formats a card number string with appropriate spacing.

```javascript { .api }
/**
 * Format card number with appropriate spacing
 *
 * Adds spaces to card number based on detected card type format.
 * Most cards use 4-4-4-4 spacing, American Express uses 4-6-5.
 *
 * @param number - Card number to format
 * @returns Formatted card number string with spaces
 */
$.payment.formatCardNumber(number: string): string;
```

**Usage Examples:**

```javascript
// Format various card types
$.payment.formatCardNumber('4242424242424242');
// '4242 4242 4242 4242'

$.payment.formatCardNumber('378282246310005');
// '3782 822463 10005' (American Express format)

$.payment.formatCardNumber('6011111111111117');
// '6011 1111 1111 1117'

// Strips non-numeric characters first
$.payment.formatCardNumber('4242-4242-4242-4242');
// '4242 4242 4242 4242'
```

**Behavior:**

- Non-numeric characters are removed before formatting
- Spacing is added based on detected card type
- Number is truncated to maximum valid length for card type
- Returns empty string if card type cannot be determined and number is empty

### Format Expiry

Programmatically formats an expiry string to MM / YYYY format.

```javascript { .api }
/**
 * Format expiry string to MM / YYYY
 *
 * Formats various expiry inputs into consistent MM / YYYY format.
 * Adds leading zeros and separator as needed.
 *
 * @param expiry - Expiry string to format
 * @returns Formatted expiry string
 */
$.payment.formatExpiry(expiry: string): string;
```

**Usage Examples:**

```javascript
// Format various inputs
$.payment.formatExpiry('0325');
// '03 / 25'

$.payment.formatExpiry('1225');
// '12 / 25'

$.payment.formatExpiry('12025');
// '12 / 025'

$.payment.formatExpiry('5');
// '5' (incomplete)

$.payment.formatExpiry('525');
// '05 / 25' (smart handling of invalid month)
```

**Behavior:**

- Automatically adds leading zero to single-digit months when appropriate
- Adds ' / ' separator between month and year
- Handles partial input gracefully
- Does not validate the date

## Card Type Extension

### $.payment.cards

The extensible array of card type definitions used for detection and validation.

```javascript { .api }
/**
 * Array of card type definitions
 *
 * Each card type object contains:
 * - type: Card type identifier string
 * - patterns: Array of numeric prefixes for identification
 * - format: RegExp for formatting the card number with spaces
 * - length: Array of valid card number lengths
 * - cvcLength: Array of valid CVC lengths
 * - luhn: Boolean indicating whether to validate using Luhn algorithm
 */
$.payment.cards: CardDefinition[];

interface CardDefinition {
  type: string;
  patterns: number[];
  format: RegExp;
  length: number[];
  cvcLength: number[];
  luhn: boolean;
}
```

**Usage Example - Adding Custom Card Type:**

```javascript
// Define custom card type
var customCard = {
  type: 'mycard',
  patterns: [8888],              // Cards starting with 8888
  format: /(\d{1,4})/g,         // Format as 4-4-4-4
  length: [16],                  // 16 digits
  cvcLength: [3],                // 3-digit CVC
  luhn: true                     // Validate with Luhn
};

// Add to beginning of array (higher priority)
$.payment.cards.unshift(customCard);

// Now custom card type will be detected
$.payment.cardType('8888111122223333');  // 'mycard'
$.payment.validateCardNumber('8888111122223333');  // validates with Luhn
```

**Built-in Card Definitions:**

The array includes definitions for all major card types:

```javascript
// Example: Visa definition
{
  type: 'visa',
  patterns: [4],
  format: /(\d{1,4})/g,
  length: [13, 16],
  cvcLength: [3],
  luhn: true
}

// Example: American Express definition
{
  type: 'amex',
  patterns: [34, 37],
  format: /(\d{1,4})(\d{1,6})?(\d{1,5})?/,
  length: [15],
  cvcLength: [3, 4],
  luhn: true
}

// Example: Maestro definition (variable length)
{
  type: 'maestro',
  patterns: [5018, 502, 503, 506, 56, 58, 639, 6220, 67],
  format: /(\d{1,4})/g,
  length: [12, 13, 14, 15, 16, 17, 18, 19],
  cvcLength: [3],
  luhn: true
}
```

**Pattern Matching:**

- Patterns are checked in array order
- More specific patterns should appear first
- First matching pattern determines the card type
- Patterns are converted to strings and matched against the card number prefix

**Format RegExp:**

- Defines how to add spaces to the card number
- Global flag (`/g`) uses `match()` and joins results
- Non-global uses `exec()` to split into groups
- Most cards use `/(\d{1,4})/g` for 4-4-4-4 spacing

## Complete Example - Custom Card Support

```javascript
// Add support for fictional "Wing" card
var wingCard = {
  type: 'wing',
  patterns: [501818],
  format: /(\d{1,4})/g,
  length: [16],
  cvcLength: [3],
  luhn: false  // No Luhn validation
};

// Add to array (at beginning for priority)
$.payment.cards.unshift(wingCard);

// Now Wing cards work throughout the library
var cardNumber = '5018181818181818';

$.payment.cardType(cardNumber);              // 'wing'
$.payment.validateCardNumber(cardNumber);    // true (no Luhn check)
$.payment.formatCardNumber(cardNumber);      // '5018 1818 1818 1818'

// Apply formatting to input
$('input.cc-number').payment('formatCardNumber');
// Input will add 'wing' CSS class when Wing card is detected
```
