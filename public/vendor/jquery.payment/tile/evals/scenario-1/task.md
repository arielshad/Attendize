# Payment Form Validator

## Problem Statement

Build a payment form validation system that handles credit card input validation with support for a custom regional card type. The system should parse, validate, and provide detailed feedback on card numbers, expiry dates, and security codes.

## Requirements

### 1. Custom Card Type Support

Implement support for a fictional regional card network called "RegionCard" with the following specifications:

- Card numbers start with the prefix `650` or `651`
- Card numbers are exactly 16 digits long
- Cards use standard Luhn algorithm validation
- CVC codes are 3 digits
- Cards should be formatted in groups of 4 digits (e.g., "6501 2345 6789 0123")

The system must correctly identify RegionCard numbers and distinguish them from other card types with overlapping prefixes.

### 2. Card Information Extractor

Create a function `getCardInfo(cardNumber)` that returns an object containing:

- `type`: The detected card type as a string (e.g., "visa", "mastercard", "regioncard", or null if unknown)
- `valid`: Boolean indicating whether the card number is valid
- `formatted`: The card number formatted with appropriate spacing

Example output:
```javascript
{
  type: "regioncard",
  valid: true,
  formatted: "6501 2345 6789 0127"
}
```

### 3. Expiry Date Validator

Create a function `validateExpiry(expiryString)` that:

- Accepts expiry dates in various formats: "MM/YY", "MM/YYYY", "MM / YY", etc.
- Parses the expiry string into month and year components
- Validates that the expiry date is in the future
- Returns an object with:
  - `valid`: Boolean indicating validity
  - `month`: Parsed month as a number
  - `year`: Parsed year as a 4-digit number

Example:
```javascript
validateExpiry("03/25")  // Returns: {valid: true, month: 3, year: 2025}
validateExpiry("01/20")  // Returns: {valid: false, month: 1, year: 2020} (if current year > 2020)
```

### 4. Security Code Validator

Create a function `validateSecurityCode(cvc, cardType)` that:

- Validates the security code based on the card type
- Handles the special case where some card types accept 4 digits
- Returns a boolean indicating validity

The validator should be aware that different card types may have different security code length requirements.

## Dependencies {.dependencies}

### jquery.payment {.dependency}

A jQuery plugin for building credit card forms with validation and formatting support.

## Test Cases

### Test 1: RegionCard Detection and Validation {.test}

**Input:**
- Card number: "6501234567890127"

**Expected Output:**
```javascript
{
  type: "regioncard",
  valid: true,
  formatted: "6501 2345 6789 0127"
}
```

### Test 2: Invalid Card Number {.test}

**Input:**
- Card number: "6501234567890128"

**Expected Output:**
```javascript
{
  type: "regioncard",
  valid: false,
  formatted: "6501 2345 6789 0128"
}
```

### Test 3: Future Expiry Date Validation {.test}

**Input:**
- Expiry string: "12/30"

**Expected Output:**
```javascript
{
  valid: true,
  month: 12,
  year: 2030
}
```

### Test 4: Past Expiry Date {.test}

**Input:**
- Expiry string: "01/20"

**Expected Output:**
```javascript
{
  valid: false,
  month: 1,
  year: 2020
}
```

### Test 5: American Express Security Code {.test}

**Input:**
- CVC: "1234"
- Card type: "amex"

**Expected Output:**
```javascript
true
```

## Implementation Notes

- Your solution should be in JavaScript and use jQuery
- Include all necessary imports and setup code
- Ensure the custom card type is properly integrated and takes precedence over conflicting patterns
- Handle edge cases such as invalid input formats gracefully

## Constraints

- All functions must handle invalid inputs without throwing errors
- Card number formatting should match the standard spacing for the detected card type
- Date validation must account for cards expiring at the end of the stated month
- The custom RegionCard type must be added to the system before processing any card numbers

## Deliverables

Implement the three required functions (`getCardInfo`, `validateExpiry`, `validateSecurityCode`) along with the necessary setup code to register the RegionCard type. Create your implementation in a file named `payment-validator.test.js`.
