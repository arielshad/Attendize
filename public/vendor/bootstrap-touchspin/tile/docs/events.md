# Events

Bootstrap TouchSpin provides a comprehensive event system for programmatic control and monitoring. Events are divided into two categories: triggerable events (commands) and observable events (notifications).

## Capabilities

### Triggerable Events (Commands)

These events can be triggered on the input element to control the spinner programmatically.

#### Increment Once

Increment the value by one step.

```javascript { .api }
/**
 * Increment value once by the configured step amount
 * Stops any active spinning before incrementing
 */
$('input').trigger('touchspin.uponce');
```

**Usage Example:**

```javascript
// Increment the value programmatically
$('#myInput').trigger('touchspin.uponce');

// Use in a custom button
$('#customUpButton').on('click', function() {
  $('#myInput').trigger('touchspin.uponce');
});
```

#### Decrement Once

Decrement the value by one step.

```javascript { .api }
/**
 * Decrement value once by the configured step amount
 * Stops any active spinning before decrementing
 */
$('input').trigger('touchspin.downonce');
```

**Usage Example:**

```javascript
// Decrement the value programmatically
$('#myInput').trigger('touchspin.downonce');

// Use in a custom button
$('#customDownButton').on('click', function() {
  $('#myInput').trigger('touchspin.downonce');
});
```

#### Start Upward Spinning

Start continuous upward spinning (repeated increments).

```javascript { .api }
/**
 * Start continuous upward spinning
 * Increments value repeatedly at the configured stepinterval
 * Stops any existing spinning before starting
 */
$('input').trigger('touchspin.startupspin');
```

**Usage Example:**

```javascript
// Start continuous upward spinning on mousedown
$('#customUpButton').on('mousedown', function() {
  $('#myInput').trigger('touchspin.startupspin');
}).on('mouseup mouseleave', function() {
  $('#myInput').trigger('touchspin.stopspin');
});
```

#### Start Downward Spinning

Start continuous downward spinning (repeated decrements).

```javascript { .api }
/**
 * Start continuous downward spinning
 * Decrements value repeatedly at the configured stepinterval
 * Stops any existing spinning before starting
 */
$('input').trigger('touchspin.startdownspin');
```

**Usage Example:**

```javascript
// Start continuous downward spinning on mousedown
$('#customDownButton').on('mousedown', function() {
  $('#myInput').trigger('touchspin.startdownspin');
}).on('mouseup mouseleave', function() {
  $('#myInput').trigger('touchspin.stopspin');
});
```

#### Stop Spinning

Stop any active spinning operation.

```javascript { .api }
/**
 * Stop any active spinning (upward or downward)
 * Clears all spin timers and resets spin state
 */
$('input').trigger('touchspin.stopspin');
```

**Usage Example:**

```javascript
// Stop spinning when user focuses another element
$('#otherInput').on('focus', function() {
  $('#myInput').trigger('touchspin.stopspin');
});
```

#### Update Settings

Update configuration options after initialization.

```javascript { .api }
/**
 * Update plugin settings after initialization
 * @param {Object} newsettings - Object containing options to update
 */
$('input').trigger('touchspin.updatesettings', newsettings);
```

**Usage Example:**

```javascript
// Update min/max range dynamically
$('#myInput').trigger('touchspin.updatesettings', {
  min: 10,
  max: 200,
  step: 5
});

// Update styling options
$('#myInput').trigger('touchspin.updatesettings', {
  prefix: '€',
  postfix: 'EUR',
  decimals: 2
});
```

### Observable Events (Notifications)

These events are triggered by the plugin and can be observed using jQuery's `.on()` method.

#### Value Change

Standard jQuery change event fired when the value changes.

```javascript { .api }
/**
 * Fired when the value changes
 * Standard jQuery change event
 */
$('input').on('change', function(e) {
  // Handle value change
});
```

**Usage Example:**

```javascript
$('#myInput').on('change', function() {
  var newValue = $(this).val();
  console.log('New value:', newValue);

  // Update display or perform validation
  $('#displayValue').text(newValue);
});
```

#### Minimum Value Reached

Fired when the minimum value is reached.

```javascript { .api }
/**
 * Fired when decrementing reaches the minimum value
 * Spinning is automatically stopped when this occurs
 */
$('input').on('touchspin.on.min', function(e) {
  // Handle minimum reached
});
```

**Usage Example:**

```javascript
$('#myInput').on('touchspin.on.min', function() {
  console.log('Minimum value reached');
  $('#minWarning').show();
});
```

#### Maximum Value Reached

Fired when the maximum value is reached.

```javascript { .api }
/**
 * Fired when incrementing reaches the maximum value
 * Spinning is automatically stopped when this occurs
 */
$('input').on('touchspin.on.max', function(e) {
  // Handle maximum reached
});
```

**Usage Example:**

```javascript
$('#myInput').on('touchspin.on.max', function() {
  console.log('Maximum value reached');
  $('#maxWarning').show();
});
```

#### Spinning Started

Fired when any spinning operation starts.

```javascript { .api }
/**
 * Fired when spinning starts (either upward or downward)
 * Triggered before touchspin.on.startupspin or touchspin.on.startdownspin
 */
$('input').on('touchspin.on.startspin', function(e) {
  // Handle spin start
});
```

**Usage Example:**

```javascript
$('#myInput').on('touchspin.on.startspin', function() {
  console.log('Spinning started');
  $(this).addClass('spinning-active');
});
```

#### Upward Spinning Started

Fired when upward spinning starts.

```javascript { .api }
/**
 * Fired when upward spinning starts
 * Triggered after touchspin.on.startspin
 */
$('input').on('touchspin.on.startupspin', function(e) {
  // Handle upward spin start
});
```

**Usage Example:**

```javascript
$('#myInput').on('touchspin.on.startupspin', function() {
  console.log('Upward spinning started');
  $('#upIndicator').addClass('active');
});
```

#### Downward Spinning Started

Fired when downward spinning starts.

```javascript { .api }
/**
 * Fired when downward spinning starts
 * Triggered after touchspin.on.startspin
 */
$('input').on('touchspin.on.startdownspin', function(e) {
  // Handle downward spin start
});
```

**Usage Example:**

```javascript
$('#myInput').on('touchspin.on.startdownspin', function() {
  console.log('Downward spinning started');
  $('#downIndicator').addClass('active');
});
```

#### Spinning Stopped

Fired when any spinning operation stops.

```javascript { .api }
/**
 * Fired when spinning stops (either upward or downward)
 * Triggered after touchspin.on.stopupspin or touchspin.on.stopdownspin
 */
$('input').on('touchspin.on.stopspin', function(e) {
  // Handle spin stop
});
```

**Usage Example:**

```javascript
$('#myInput').on('touchspin.on.stopspin', function() {
  console.log('Spinning stopped');
  $(this).removeClass('spinning-active');
});
```

#### Upward Spinning Stopped

Fired when upward spinning stops.

```javascript { .api }
/**
 * Fired when upward spinning stops
 * Triggered before touchspin.on.stopspin
 */
$('input').on('touchspin.on.stopupspin', function(e) {
  // Handle upward spin stop
});
```

**Usage Example:**

```javascript
$('#myInput').on('touchspin.on.stopupspin', function() {
  console.log('Upward spinning stopped');
  $('#upIndicator').removeClass('active');
});
```

#### Downward Spinning Stopped

Fired when downward spinning stops.

```javascript { .api }
/**
 * Fired when downward spinning stops
 * Triggered before touchspin.on.stopspin
 */
$('input').on('touchspin.on.stopdownspin', function(e) {
  // Handle downward spin stop
});
```

**Usage Example:**

```javascript
$('#myInput').on('touchspin.on.stopdownspin', function() {
  console.log('Downward spinning stopped');
  $('#downIndicator').removeClass('active');
});
```

## Complete Event Example

```javascript
// Initialize TouchSpin
$('#quantity').TouchSpin({
  min: 1,
  max: 100,
  step: 1
});

// Listen to all events
$('#quantity')
  .on('change', function() {
    console.log('Value changed to:', $(this).val());
  })
  .on('touchspin.on.min', function() {
    console.log('Minimum reached');
    $('#decreaseBtn').prop('disabled', true);
  })
  .on('touchspin.on.max', function() {
    console.log('Maximum reached');
    $('#increaseBtn').prop('disabled', true);
  })
  .on('touchspin.on.startspin', function() {
    $(this).addClass('spinning');
  })
  .on('touchspin.on.stopspin', function() {
    $(this).removeClass('spinning');
    $('#decreaseBtn').prop('disabled', false);
    $('#increaseBtn').prop('disabled', false);
  });

// Programmatic control
$('#increaseBtn').on('click', function() {
  $('#quantity').trigger('touchspin.uponce');
});

$('#decreaseBtn').on('click', function() {
  $('#quantity').trigger('touchspin.downonce');
});

$('#resetBtn').on('click', function() {
  $('#quantity').val(50).trigger('change');
});
```

## Event Flow

### Single Increment/Decrement

1. `touchspin.uponce` or `touchspin.downonce` triggered
2. Value changes (if within bounds)
3. `change` event fires (if value actually changed)
4. `touchspin.on.min` or `touchspin.on.max` fires (if boundary reached)

### Continuous Spinning

**Start:**
1. `touchspin.startupspin` or `touchspin.startdownspin` triggered
2. `touchspin.on.startspin` fires
3. `touchspin.on.startupspin` or `touchspin.on.startdownspin` fires
4. Value changes repeatedly with `change` events

**Stop:**
1. `touchspin.stopspin` triggered (or boundary reached, or user releases)
2. `touchspin.on.stopupspin` or `touchspin.on.stopdownspin` fires
3. `touchspin.on.stopspin` fires
