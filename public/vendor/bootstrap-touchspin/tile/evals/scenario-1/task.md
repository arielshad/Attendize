# Dynamic Range Adjuster

Build a web form component that allows users to adjust a numeric quantity with intelligent range limits that change based on context.

## Requirements

Your solution should create an HTML form with an interactive numeric input that has the following behaviors:

### Basic Setup

Create an HTML page with a numeric input field that allows users to select a quantity between 0 and 100, starting at 50. The input should have increment and decrement buttons.

### Dynamic Range Adjustment

When the user reaches or exceeds a quantity of 75, the maximum allowed value should automatically increase to 200. This adjustment should happen seamlessly without resetting the current value.

### Boundary Notifications

The system should track when users hit the minimum or maximum boundaries. Display a message on the page showing:
- How many times the minimum boundary (0) was reached
- How many times the maximum boundary was reached

These counters should persist across the entire user session and increment each time a boundary is hit.

### Acceleration for Large Ranges

When the maximum limit is 200, users should be able to quickly traverse the larger range. The increment/decrement speed should accelerate as users hold down the buttons, but the acceleration should not exceed steps of 10 at a time.

### Test Cases

Create a test file `adjuster.test.js` with the following test cases:

- The input initializes with value 50 [@test](../test/adjuster.test.js)
- Incrementing from 50 reaches 75 [@test](../test/adjuster.test.js)
- When value reaches 75, the maximum limit becomes 200 [@test](../test/adjuster.test.js)
- Users can increment beyond 75 to reach higher values [@test](../test/adjuster.test.js)

## Implementation

[@generates](./src/adjuster.html)

## Dependencies { .dependencies }

### bootstrap-touchspin { .dependency }

Provides an interactive numeric spinner component with increment/decrement buttons.
