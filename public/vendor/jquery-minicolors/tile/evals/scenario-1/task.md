# Theme Customizer Widget

Build a theme customization widget that allows users to configure a website's color scheme with preset palettes and custom colors.

## Requirements

### Theme Manager Initialization

The widget should initialize a color picker with a preset palette of theme colors. The palette should include these colors:
- Primary: `#3498db`
- Secondary: `#2ecc71`
- Accent: `#e74c3c`
- Dark: `#34495e`
- Light: `#ecf0f1`

The color picker should be configured to:
- Display in RGB format
- Enable opacity control
- Use the wheel control type
- Show colors with uppercase letters when in hex format
- Position the dropdown at the top right

### Dynamic Mode Switching

Implement functionality to switch between "Simple Mode" and "Advanced Mode":

**Simple Mode:**
- Uses default hue control
- Displays colors in hex format
- Hides opacity control

**Advanced Mode:**
- Uses wheel control
- Displays colors in RGB format
- Shows opacity control

### Preset Color Application

When a user clicks on a preset palette color, the widget should:
- Apply both the color and a default opacity of 0.85
- Update the color picker to reflect the selection

### Color Information Display

Create a function that retrieves the current color value and returns an object containing:
- The RGB values as an object with r, g, b, and a properties
- The color formatted as an RGBA CSS string

This information should be used to update a preview element showing the selected color.

## Implementation

[@generates](./src/theme-customizer.js)

## API

```javascript { #api }
/**
 * Initializes the theme customizer widget on the specified input element
 * @param {string} selector - jQuery selector for the input element
 */
function initThemeCustomizer(selector) {
  // Initialize with preset palette and configuration
}

/**
 * Switches the color picker between simple and advanced modes
 * @param {string} selector - jQuery selector for the input element
 * @param {string} mode - Either 'simple' or 'advanced'
 */
function switchMode(selector, mode) {
  // Update settings based on mode
}

/**
 * Applies a preset color with opacity to the color picker
 * @param {string} selector - jQuery selector for the input element
 * @param {string} color - The color value to apply
 */
function applyPresetColor(selector, color) {
  // Apply color with default opacity of 0.85
}

/**
 * Gets the current color information
 * @param {string} selector - jQuery selector for the input element
 * @returns {Object} Object containing rgbObject and cssString properties
 */
function getColorInfo(selector) {
  // Return current color as object and CSS string
}
```

## Dependencies { .dependencies }

### jquery-minicolors { .dependency }

A jQuery color picker plugin with support for RGB, hex formats, opacity, swatches, and multiple control types.

## Tests

- Initializes the color picker with preset swatches and wheel control [@test](../test/theme-customizer.test.js)
- Switches from advanced mode to simple mode and updates format to hex [@test](../test/theme-customizer.test.js)
- Applies a preset color with opacity 0.85 using object notation [@test](../test/theme-customizer.test.js)
- Retrieves color information as both RGB object and RGBA string [@test](../test/theme-customizer.test.js)
