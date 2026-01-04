# Location Finder with Fine-Tuning

Build an interactive location search interface with map integration, draggable marker adjustment, and comprehensive address details capture.

## Requirements

Create a location search page that allows users to:
- Search for locations with autocomplete suggestions
- View the selected location on an interactive map
- Fine-tune the exact position by dragging the marker
- Reset to the originally geocoded position if needed
- Capture detailed address components in a form

## Capabilities

### Location Search with Autocomplete

- When a user types into the search input, autocomplete suggestions appear based on their input [@test](../test/autocomplete.test.js)
- When a user selects a suggestion, the map centers on that location and displays a marker [@test](../test/selection.test.js)
- When multiple geocoding results are returned, the first result is automatically used [@test](../test/default-result.test.js)

### Map Integration

- The map displays centered on the geocoded location after a successful search [@test](../test/map-center.test.js)
- The marker position updates to match the geocoded location [@test](../test/marker-position.test.js)

### Manual Position Adjustment

- When the user drags the marker to a new position, the latitude and longitude fields update with the new coordinates [@test](../test/marker-drag.test.js)
- After dragging the marker, a "Reset to Original" button becomes visible [@test](../test/reset-button-visibility.test.js)
- When the "Reset to Original" button is clicked, the marker returns to the last geocoded position and coordinates reset [@test](../test/reset-position.test.js)

### Address Details Capture

- After a successful geocode, the form fields populate with structured address data including street address, city, state, postal code, country (full name), and country code [@test](../test/form-population.test.js)
- The latitude and longitude fields display the coordinates of the selected location [@test](../test/coordinates-display.test.js)

## Implementation

Create an HTML page (`index.html`) with:
- A search input field with id `geocomplete`
- A map container with id `map`
- A form with id `address-form` containing inputs for:
  - `lat` (latitude)
  - `lng` (longitude)
  - `formatted_address` (full address)
  - `route` (street name)
  - `locality` (city)
  - `administrative_area_level_1` (state/province)
  - `postal_code` (zip/postal code)
  - `country` (country full name)
  - `country_short` (country code)
- A reset button with id `reset-btn` (initially hidden)
- Required JavaScript libraries (jQuery, Google Maps API with Places library)

The implementation should use appropriate configuration for:
- Enabling marker dragging functionality
- Linking the search input to both the map and form
- Managing the visibility of the reset button
- Handling marker position changes
- Restoring original geocoded positions

[@generates](./index.html)

## Dependencies { .dependencies }

### geocomplete { .dependency }

Provides jQuery geocoding and places autocomplete functionality.

The plugin wraps Google Maps API's Geocoding and Places Autocomplete services to enable location search with autocomplete, map integration, and form population capabilities.
