# Card Validation

Client-side validation functions for credit card numbers, expiry dates, and CVC codes. These static functions can be called programmatically to validate payment form inputs before submission.

## Capabilities

### Validate Card Number

Validates a credit card number using the Luhn algorithm and checks against known card type patterns and lengths.

```javascript { .api }
/**
 * Validate a credit card number
 *
 * Validation checks:
 * - Removes spaces and dashes, validates remaining characters are numeric
 * - Matches number against known card type patterns
 * - Validates length is valid for the detected card type
 * - Validates Luhn checksum (for card types that use Luhn)
 *
 * @param number - Card number string (spaces and dashes are allowed)
 * @returns true if valid, false otherwise
 */
$.payment.validateCardNumber(number: string): boolean;
```

**Usage Examples:**

```javascript
// Valid card numbers
$.payment.validateCardNumber('4242424242424242');     // true (Visa)
$.payment.validateCardNumber('4242 4242 4242 4242'); // true (spaces allowed)
$.payment.validateCardNumber('4242-4242-4242-4242'); // true (dashes allowed)
$.payment.validateCardNumber('378282246310005');     // true (Amex)
$.payment.validateCardNumber('5555555555554444');    // true (Mastercard)

// Invalid card numbers
$.payment.validateCardNumber('4242424242424241');    // false (fails Luhn)
$.payment.validateCardNumber('42424242424242424');   // false (too long)
$.payment.validateCardNumber('424242424');           // false (too short)
$.payment.validateCardNumber('4242424e42424242');    // false (contains letter)
$.payment.validateCardNumber('');                    // false (empty)
```

**Supported Card Types:**

The function validates all card types defined in `$.payment.cards`:

- Visa: 13 or 16 digits, begins with 4
- MasterCard: 16 digits, begins with 51-55 or 22-27
- American Express: 15 digits, begins with 34 or 37
- Diners Club: 14 digits, begins with 30, 36, 38, or 39
- Discover: 16 digits, begins with 60, 64, 65, or 622
- UnionPay: 16-19 digits, begins with 62 or 88 (no Luhn check)
- JCB: 16 digits, begins with 35
- Visa Electron: 16 digits, specific prefixes
- Maestro: 12-19 digits, various prefixes
- Forbrugsforeningen: 16 digits, begins with 600
- Dankort: 16 digits, begins with 5019
- Elo: 16 digits, various prefixes

### Validate Card Expiry

Validates that a card expiry date is valid and in the future.

```javascript { .api }
/**
 * Validate a card expiry date
 *
 * Validation checks:
 * - Month is between 1 and 12
 * - Year is numeric and valid
 * - Date is in the future (or current month)
 * - Supports 2-digit or 4-digit years
 *
 * @param month - Month (1-12) or object with month and year properties
 * @param year - Year (2 or 4 digits, optional if first param is object)
 * @returns true if valid and in future, false otherwise
 */
$.payment.validateCardExpiry(month: string | number | object, year?: string | number): boolean;
```

**Usage Examples:**

```javascript
// Standard validation with month and year
$.payment.validateCardExpiry('05', '2025');   // true (if current date is before May 2025)
$.payment.validateCardExpiry('12', '25');     // true (2-digit year supported)
$.payment.validateCardExpiry(5, 2025);        // true (numbers supported)
$.payment.validateCardExpiry('05', '2015');   // false (past year)
$.payment.validateCardExpiry('13', '2025');   // false (invalid month)

// Object format
var expiry = $.payment.cardExpiryVal('03 / 2025');
$.payment.validateCardExpiry(expiry);         // true (object with month/year)

// Current month and year
var now = new Date();
var currentMonth = now.getMonth() + 1;
var currentYear = now.getFullYear();
$.payment.validateCardExpiry(currentMonth, currentYear); // true (current month is valid)
```

**Year Handling:**

- 4-digit years are used as-is
- 2-digit years < 70 are interpreted as 20xx (e.g., 25 → 2025)
- 2-digit years >= 70 are interpreted as 19xx (e.g., 99 → 1999)
- Years must be exactly 2 or 4 digits

### Validate Card CVC

Validates a CVC/CVV code based on length requirements.

```javascript { .api }
/**
 * Validate a CVC/CVV code
 *
 * Validation checks:
 * - Contains only numeric characters
 * - Length is 3-4 digits (4 for Amex, 3 for most others)
 * - If card type provided, validates against type-specific requirements
 *
 * @param cvc - CVC code string
 * @param type - Optional card type ('amex', 'visa', etc.)
 * @returns true if valid, false otherwise
 */
$.payment.validateCardCVC(cvc: string, type?: string): boolean;
```

**Usage Examples:**

```javascript
// Without card type (validates 3-4 digits)
$.payment.validateCardCVC('123');      // true
$.payment.validateCardCVC('1234');     // true
$.payment.validateCardCVC('12');       // false (too short)
$.payment.validateCardCVC('12345');    // false (too long)

// With card type
$.payment.validateCardCVC('123', 'visa');       // true (Visa requires 3)
$.payment.validateCardCVC('1234', 'visa');      // false (Visa only allows 3)
$.payment.validateCardCVC('123', 'amex');       // true (Amex allows 3 or 4)
$.payment.validateCardCVC('1234', 'amex');      // true (Amex allows 3 or 4)
$.payment.validateCardCVC('123', 'mastercard'); // true

// Invalid input
$.payment.validateCardCVC('12e');      // false (non-numeric)
$.payment.validateCardCVC('');         // false (empty)
```

**Card Type CVC Requirements:**

- American Express: 3 or 4 digits
- All other card types: 3 digits
- Without card type specified: 3 or 4 digits accepted

## Complete Validation Example

```javascript
// Validate complete payment form
function validatePaymentForm() {
  var cardNumber = $('input.cc-number').val();
  var expiry = $('input.cc-expiry').payment('cardExpiryVal');
  var cvc = $('input.cc-cvc').val();

  // Get card type for CVC validation
  var cardType = $.payment.cardType(cardNumber);

  // Validate all fields
  var cardValid = $.payment.validateCardNumber(cardNumber);
  var expiryValid = $.payment.validateCardExpiry(expiry);
  var cvcValid = $.payment.validateCardCVC(cvc, cardType);

  // Display errors
  if (!cardValid) {
    showError('card-number', 'Invalid card number');
  }
  if (!expiryValid) {
    showError('expiry', 'Invalid or expired date');
  }
  if (!cvcValid) {
    showError('cvc', 'Invalid security code');
  }

  return cardValid && expiryValid && cvcValid;
}

// Submit form
$('form').submit(function(e) {
  if (!validatePaymentForm()) {
    e.preventDefault();
    return false;
  }
  // Proceed with form submission
});
```

## Important Notes

### Client-Side Only

These validation functions are for client-side validation only to improve user experience. Always validate payment information on the server as well for security.

### Luhn Algorithm

The Luhn algorithm (mod 10 checksum) is used for most card types to validate the card number mathematically. UnionPay cards do not use Luhn validation.

### Expiry Date Precision

Expiry validation considers a card valid through the last day of the expiry month. For example, a card expiring "03/2025" is valid through March 31, 2025.

### No PCI Compliance

This library only handles formatting and validation. Proper handling of credit card data for PCI compliance (encryption, transmission, storage) must be implemented separately. Never store CVC codes.
