# Geocomplete

Geocomplete is a jQuery plugin that wraps the Google Maps API's Geocoding and Places Autocomplete services, transforming standard HTML input fields into location search interfaces with autocomplete functionality. It provides automatic address completion with dropdown suggestions, optional interactive map integration with customizable markers and zoom controls, automatic population of form fields with structured address components (street, city, postal code, coordinates, etc.), support for multiple geocoding result handling, and draggable markers for manual location adjustment.

## Package Information

- **Package Name**: geocomplete
- **Package Type**: npm
- **Language**: JavaScript
- **Installation**: `npm install geocomplete`
- **Dependencies**: jQuery (1.9.0+), Google Maps JavaScript API with Places Library

## Loading the Plugin

Before using geocomplete, you must include jQuery and the Google Maps JavaScript API with the Places Library:

```html
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
<script src="https://maps.googleapis.com/maps/api/js?libraries=places&key=YOUR_API_KEY"></script>
<script src="node_modules/geocomplete/jquery.geocomplete.js"></script>
```

**Note**: If using the plugin without showing a map, you must display the "[powered by Google](https://developers.google.com/maps/documentation/javascript/places#autocomplete_no_map)" logo under the text field per Google's requirements.

## Basic Usage

```javascript
// Simple autocomplete on an input field
$("input").geocomplete();

// With options
$("#geocomplete").geocomplete({
  map: "#map",
  details: "form",
  types: ['geocode']
});

// Trigger geocoding programmatically
$("#geocomplete").trigger("geocode");

// Listen for results
$("#geocomplete")
  .geocomplete()
  .bind("geocode:result", function(event, result){
    console.log("Location:", result.formatted_address);
    console.log("Coordinates:", result.geometry.location.lat(), result.geometry.location.lng());
  });
```

## Capabilities

### Plugin Initialization

Initialize the geocomplete plugin on a jQuery-selected input element.

```javascript { .api }
/**
 * Initialize geocomplete plugin with options
 * @param {Object} options - Configuration options
 * @returns {jQuery} jQuery object for chaining
 */
$.fn.geocomplete(options);

/**
 * Call methods or access properties on initialized instance
 * @param {string} methodName - Method name or property name
 * @param {...*} args - Arguments for the method
 * @returns {jQuery|*} jQuery object for chaining or property value
 */
$.fn.geocomplete(methodName, ...args);
```

**Usage Examples:**

```javascript
// Initialize with default options
$("input").geocomplete();

// Initialize with custom options
$("#address").geocomplete({
  map: ".map-canvas",
  details: "form",
  location: "New York, NY"
});

// Call a method on initialized instance
$("#address").geocomplete("find", "Boston, MA");

// Access a property
var map = $("#address").geocomplete("map");
map.setZoom(12);
```

### Configuration Options

All configuration options that can be passed to the plugin initialization.

```javascript { .api }
interface GeoCompleteOptions {
  /** Map container - selector, jQuery object, DOM element, or false to disable. Default: false */
  map?: string | jQuery | HTMLElement | false;

  /** Container to populate with geocoded data - selector, jQuery object, DOM element, or false. Default: false */
  details?: string | jQuery | HTMLElement | false;

  /** Initial location - address string, [lat, lng] array, google.maps.LatLng object, or false. Default: false */
  location?: string | [number, number] | google.maps.LatLng | false;

  /** Snap geocode search to map bounds - true for map bounds, false for global, or LatLngBounds object. Default: true */
  bounds?: boolean | google.maps.LatLngBounds;

  /** Country code for restricting results (e.g., "us", "uk"). Default: null */
  country?: string | null;

  /** Attribute name to use for mapping detail fields. Default: "name" */
  detailsAttribute?: string;

  /** Automatically select highlighted or first item on Enter. Default: true */
  autoselect?: boolean;

  /** Options for google.maps.Map constructor. Default: {zoom: 14, scrollwheel: false, mapTypeId: "roadmap"} */
  mapOptions?: {
    /** Initial zoom level. Default: 14 */
    zoom?: number;
    /** Enable scrollwheel zoom. Default: false */
    scrollwheel?: boolean;
    /** Map type ID. Default: "roadmap" */
    mapTypeId?: string;
    /** Additional google.maps.MapOptions properties */
    [key: string]: any;
  };

  /** Options for google.maps.Marker constructor. Default: {draggable: false} */
  markerOptions?: {
    /** Whether marker is draggable. Default: false */
    draggable?: boolean;
    /** Do not show marker. Default: false */
    disabled?: boolean;
    /** Marker animation (e.g., google.maps.Animation.DROP) */
    animation?: google.maps.Animation;
    /** Additional google.maps.MarkerOptions properties */
    [key: string]: any;
  };

  /** Maximum zoom level after geocoding response. Default: 16 */
  maxZoom?: number;

  /** Place types for autocomplete request. Default: ['geocode'] */
  types?: string[];

  /** Component restrictions for Places Autocomplete (e.g., {country: "us"}). Default: undefined */
  componentRestrictions?: {
    country?: string | string[];
  };

  /** Trigger geocoding when input loses focus. Default: false */
  blur?: boolean;

  /** If blur is true, whether to geocode if user selected a result before blur. Default: false */
  geocodeAfterResult?: boolean;

  /** Restore input's original value on blur. Default: false */
  restoreValueAfterBlur?: boolean;
}
```

**Common Option Combinations:**

```javascript
// Autocomplete only (no map)
$("input").geocomplete();

// With map display
$("#search").geocomplete({
  map: "#map-canvas"
});

// With form field population
$("#address").geocomplete({
  details: "form",
  detailsAttribute: "name"
});

// With custom attributes for form fields
$("#address").geocomplete({
  details: ".address-fields",
  detailsAttribute: "data-geo"
});

// With initial location and map
$("#search").geocomplete({
  map: "#map",
  location: [40.7128, -74.0060] // NYC coordinates
});

// With country restriction
$("#search").geocomplete({
  componentRestrictions: {
    country: "us"
  }
});

// With draggable marker
$("#search").geocomplete({
  map: "#map",
  markerOptions: {
    draggable: true
  }
});

// With blur-triggered geocoding
$("#search").geocomplete({
  blur: true,
  geocodeAfterResult: true
});

// With custom map options
$("#search").geocomplete({
  map: "#map",
  mapOptions: {
    zoom: 18,
    scrollwheel: true,
    mapTypeId: "satellite"
  }
});
```

### Geocoding Methods

Methods for performing geocoding operations.

```javascript { .api }
/**
 * Perform geocoding lookup for given address or current input value
 * @param {string} [address] - Address to geocode. Uses input value if omitted
 */
find(address);

/**
 * Request geocoding with custom Google Maps Geocoder parameters
 * @param {Object} request - Geocoding request object (google.maps.GeocoderRequest)
 */
geocode(request);

/**
 * Update the plugin with a geocoding result (triggers geocode:result event and updates map/details)
 * @param {Object} result - Geocoding result object (google.maps.GeocoderResult)
 */
update(result);
```

**Usage Examples:**

```javascript
// Geocode current input value
$("#address").geocomplete("find");

// Geocode specific address
$("#address").geocomplete("find", "1600 Amphitheatre Parkway, Mountain View, CA");

// Geocode with custom request parameters
$("#address").geocomplete("geocode", {
  address: "Paris, France",
  region: "fr"
});

// Trigger via event (alternative to calling find)
$("#address").trigger("geocode");

// External trigger button
$("button.search").click(function(){
  $("#address").trigger("geocode");
});

// Update with a specific result (useful for handling multiple results)
$("#address").geocomplete("update", result);
```

### Marker Control

Methods for controlling the map marker.

```javascript { .api }
/**
 * Reset marker position to last known geocoded location
 */
resetMarker();
```

**Usage Example:**

```javascript
// Reset marker to original position after user dragged it
$("#reset-button").click(function(){
  $("#address").geocomplete("resetMarker");
});
```

### Cleanup

Methods for destroying the plugin instance.

```javascript { .api }
/**
 * Destroy the plugin instance and clean up event listeners
 */
destroy();
```

**Usage Example:**

```javascript
// Clean up when removing the input from DOM
$("#address").geocomplete("destroy");
$("#address").remove();
```

### Property Access

Access Google Maps API objects for advanced customization.

```javascript { .api }
/**
 * Access plugin properties by name
 * @param {string} propertyName - Property name to access
 * @returns {*} Property value
 */
$.fn.geocomplete(propertyName);

// Available properties:
// - map: google.maps.Map instance
// - marker: google.maps.Marker instance
// - autocomplete: google.maps.places.Autocomplete instance
// - geocoder: google.maps.Geocoder instance
// - details: Object containing mapped detail elements
// - data: Object containing last geocoded result data
```

**Usage Examples:**

```javascript
// Get map instance and customize
var map = $("#address").geocomplete("map");
map.setZoom(15);
map.setMapTypeId("satellite");

// Get marker instance and customize
var marker = $("#address").geocomplete("marker");
marker.setAnimation(google.maps.Animation.BOUNCE);

// Get last geocoded data
var data = $("#address").geocomplete("data");
console.log("Latitude:", data.lat);
console.log("Longitude:", data.lng);
console.log("Country:", data.country);

// Get autocomplete instance
var autocomplete = $("#address").geocomplete("autocomplete");
autocomplete.setTypes(['establishment']);

// Get geocoder instance
var geocoder = $("#address").geocomplete("geocoder");
```

### Events

Events triggered by the plugin that can be bound using jQuery's event system.

```javascript { .api }
/**
 * Bind event handlers to geocomplete events
 */

// geocode:result - Fired when geocoding succeeds
// Parameters: event (jQuery.Event), result (google.maps.GeocoderResult)
$("input").bind("geocode:result", function(event, result) {});

// geocode:error - Fired when geocoding fails
// Parameters: event (jQuery.Event), status (google.maps.GeocoderStatus)
$("input").bind("geocode:error", function(event, status) {});

// geocode:multiple - Fired when multiple results are returned
// Parameters: event (jQuery.Event), results (google.maps.GeocoderResult[])
$("input").bind("geocode:multiple", function(event, results) {});

// geocode:dragged - Fired when marker is dragged to new position
// Parameters: event (jQuery.Event), latLng (google.maps.LatLng)
$("input").bind("geocode:dragged", function(event, latLng) {});

// geocode:click - Fired when map is clicked
// Parameters: event (jQuery.Event), latLng (google.maps.LatLng)
$("input").bind("geocode:click", function(event, latLng) {});

// geocode:zoom - Fired when map zoom level changes
// Parameters: event (jQuery.Event), zoom (number)
$("input").bind("geocode:zoom", function(event, zoom) {});
```

**Event Usage Examples:**

```javascript
// Handle successful geocoding
$("#address")
  .geocomplete()
  .bind("geocode:result", function(event, result){
    console.log("Found location:", result.formatted_address);
    console.log("Coordinates:", result.geometry.location.lat(), result.geometry.location.lng());
    console.log("Full result:", result);
  });

// Handle geocoding errors
$("#address")
  .geocomplete()
  .bind("geocode:error", function(event, status){
    console.error("Geocoding failed:", status);
    if (status === google.maps.GeocoderStatus.ZERO_RESULTS) {
      alert("No results found");
    } else if (status === google.maps.GeocoderStatus.OVER_QUERY_LIMIT) {
      alert("Query limit exceeded");
    }
  });

// Handle multiple results
$("#address")
  .geocomplete()
  .bind("geocode:multiple", function(event, results){
    console.log("Found", results.length, "results");
    // Display all results to user for selection
    results.forEach(function(result, index){
      console.log((index + 1) + ":", result.formatted_address);
    });
  });

// Handle marker dragging
$("#address")
  .geocomplete({
    map: "#map",
    markerOptions: {
      draggable: true
    }
  })
  .bind("geocode:dragged", function(event, latLng){
    console.log("Marker dragged to:", latLng.lat(), latLng.lng());
    // Optionally reverse geocode the new position
    $(this).geocomplete("geocode", {
      location: latLng
    });
  });

// Handle map clicks
$("#address")
  .geocomplete({
    map: "#map"
  })
  .bind("geocode:click", function(event, latLng){
    console.log("Map clicked at:", latLng.lat(), latLng.lng());
    // Move marker to clicked position
    var marker = $(this).geocomplete("marker");
    marker.setPosition(latLng);
  });

// Handle zoom changes
$("#address")
  .geocomplete({
    map: "#map"
  })
  .bind("geocode:zoom", function(event, zoom){
    console.log("Zoom level changed to:", zoom);
  });

// Chain multiple event handlers
$("#address")
  .geocomplete()
  .bind("geocode:result", function(event, result){
    $("#lat").val(result.geometry.location.lat());
    $("#lng").val(result.geometry.location.lng());
  })
  .bind("geocode:error", function(event, status){
    alert("Geocoding failed: " + status);
  });
```

### Form Field Population

Automatically populate form fields with geocoded address components.

The `details` option specifies a container (selector, jQuery object, or DOM element) whose child elements will be populated with geocoded data. The plugin uses the `detailsAttribute` option (default: `"name"`) to match elements to address components.

**Supported Address Components:**

Geocoding components: `street_address`, `route`, `intersection`, `political`, `country`, `administrative_area_level_1`, `administrative_area_level_2`, `administrative_area_level_3`, `colloquial_area`, `locality`, `sublocality`, `neighborhood`, `premise`, `subpremise`, `postal_code`, `natural_feature`, `airport`, `park`, `point_of_interest`, `post_box`, `street_number`, `floor`, `room`, `lat`, `lng`, `viewport`, `location`, `formatted_address`, `location_type`, `bounds`

Each component also has a `_short` variant for short names (e.g., `country_short`).

Places API components: `id`, `place_id`, `url`, `website`, `vicinity`, `reference`, `name`, `rating`, `international_phone_number`, `icon`, `formatted_phone_number`

**Form Population Examples:**

```html
<!-- Using default 'name' attribute -->
<input id="geocomplete" type="text" placeholder="Type an address..." />
<form>
  <input name="lat" type="text" />
  <input name="lng" type="text" />
  <input name="formatted_address" type="text" />
  <input name="street_number" type="text" />
  <input name="route" type="text" />
  <input name="locality" type="text" />
  <input name="administrative_area_level_1" type="text" />
  <input name="postal_code" type="text" />
  <input name="country" type="text" />
  <input name="country_short" type="text" />
</form>

<script>
$("#geocomplete").geocomplete({
  details: "form"
});
</script>
```

```html
<!-- Using custom data attribute -->
<input id="geocomplete" type="text" />
<div class="address-details">
  <div>Latitude: <span data-geo="lat"></span></div>
  <div>Longitude: <span data-geo="lng"></span></div>
  <div>Address: <span data-geo="formatted_address"></span></div>
  <div>City: <span data-geo="locality"></span></div>
  <div>State: <span data-geo="administrative_area_level_1"></span></div>
  <div>ZIP: <span data-geo="postal_code"></span></div>
  <div>Country: <span data-geo="country"></span></div>
  <div>Country Code: <span data-geo="country_short"></span></div>
</div>

<script>
$("#geocomplete").geocomplete({
  details: ".address-details",
  detailsAttribute: "data-geo"
});
</script>
```

```html
<!-- Complete address form example -->
<input id="address" type="text" placeholder="Search for a location..." />
<form id="address-form">
  <label>Street Number: <input name="street_number" type="text" /></label>
  <label>Street: <input name="route" type="text" /></label>
  <label>City: <input name="locality" type="text" /></label>
  <label>State: <input name="administrative_area_level_1_short" type="text" /></label>
  <label>ZIP Code: <input name="postal_code" type="text" /></label>
  <label>Country: <input name="country" type="text" /></label>
  <label>Latitude: <input name="lat" type="text" readonly /></label>
  <label>Longitude: <input name="lng" type="text" readonly /></label>
</form>
<div id="map" style="width: 100%; height: 400px;"></div>

<script>
$("#address").geocomplete({
  map: "#map",
  details: "#address-form",
  markerOptions: {
    draggable: true
  }
}).bind("geocode:dragged", function(event, latLng){
  $("#address-form input[name=lat]").val(latLng.lat());
  $("#address-form input[name=lng]").val(latLng.lng());
});
</script>
```

### Map Integration

Display geocoded locations on an interactive Google Map.

```javascript
// Basic map integration
$("#address").geocomplete({
  map: "#map-canvas"
});

// Map with custom options
$("#address").geocomplete({
  map: "#map-canvas",
  mapOptions: {
    zoom: 16,
    scrollwheel: true,
    mapTypeId: "hybrid",
    streetViewControl: true,
    mapTypeControl: true
  }
});

// Map with draggable marker
$("#address").geocomplete({
  map: "#map-canvas",
  markerOptions: {
    draggable: true,
    animation: google.maps.Animation.DROP
  }
}).bind("geocode:dragged", function(event, latLng){
  console.log("New position:", latLng.lat(), latLng.lng());
  // Optionally reverse geocode
  $(this).geocomplete("geocode", { location: latLng });
});

// Hide marker on map
$("#address").geocomplete({
  map: "#map-canvas",
  markerOptions: {
    disabled: true
  }
});

// Use existing map instance
var existingMap = new google.maps.Map(document.getElementById("map"), {
  zoom: 12,
  center: {lat: 37.7749, lng: -122.4194}
});

$("#address").geocomplete({
  map: existingMap
});
```

### Advanced Usage Patterns

**Programmatic Geocoding with Custom Parameters:**

```javascript
$("#address").geocomplete();

// Geocode with region biasing
$("#address").geocomplete("geocode", {
  address: "Springfield",
  region: "us"
});

// Geocode by coordinates
$("#address").geocomplete("geocode", {
  location: new google.maps.LatLng(40.7128, -74.0060)
});

// Geocode with bounds restriction
var bounds = new google.maps.LatLngBounds(
  new google.maps.LatLng(40.477399, -74.259090),
  new google.maps.LatLng(40.917577, -73.700272)
);
$("#address").geocomplete("geocode", {
  address: "New York",
  bounds: bounds
});
```

**Handling Multiple Results:**

```javascript
var $input = $("#address").geocomplete();
var results = [];

$input.bind("geocode:multiple", function(event, resultsArray){
  results = resultsArray;

  // Show results to user
  var $list = $("#results-list").empty();
  results.forEach(function(result, index){
    $list.append(
      $("<li>").text(result.formatted_address).click(function(){
        // User selected this result
        $input.geocomplete("update", result);
        $list.hide();
      })
    );
  });
  $list.show();
});
```

**Dynamic Location Updates:**

```javascript
$("#address").geocomplete({
  map: "#map",
  location: [37.7749, -122.4194] // Initial location (San Francisco)
});

// Update location later
setTimeout(function(){
  $("#address").geocomplete("find", "Los Angeles, CA");
}, 5000);
```

**Restricting Autocomplete by Type:**

```javascript
// Only show addresses (no landmarks or businesses)
$("#address").geocomplete({
  types: ['geocode']
});

// Only show establishments (businesses, landmarks)
$("#business").geocomplete({
  types: ['establishment']
});

// Multiple types
$("#place").geocomplete({
  types: ['(cities)', '(regions)']
});
```

**Using with Country Restrictions:**

```javascript
// Restrict to single country
$("#address").geocomplete({
  country: "us"
});

// Restrict using componentRestrictions (recommended)
$("#address").geocomplete({
  componentRestrictions: {
    country: "us"
  }
});

// Multiple countries
$("#address").geocomplete({
  componentRestrictions: {
    country: ["us", "ca", "mx"]
  }
});
```

**Blur-triggered Geocoding:**

```javascript
// Geocode when user tabs out of field
$("#address").geocomplete({
  blur: true
});

// Don't geocode on blur if user already selected a result
$("#address").geocomplete({
  blur: true,
  geocodeAfterResult: true
});

// Restore previous value on blur if result was selected
$("#address").geocomplete({
  blur: true,
  restoreValueAfterBlur: true
});
```

**Accessing and Manipulating Google Maps Objects:**

```javascript
$("#address").geocomplete({
  map: "#map",
  markerOptions: {
    draggable: true
  }
});

// Get map instance and add custom controls
var map = $("#address").geocomplete("map");
var customControl = document.createElement("div");
customControl.innerHTML = "My Control";
map.controls[google.maps.ControlPosition.TOP_RIGHT].push(customControl);

// Get marker and add info window
var marker = $("#address").geocomplete("marker");
var infoWindow = new google.maps.InfoWindow({
  content: "Selected Location"
});
marker.addListener("click", function(){
  infoWindow.open(map, marker);
});

// Get autocomplete instance and modify options
var autocomplete = $("#address").geocomplete("autocomplete");
autocomplete.setTypes(['establishment']);
autocomplete.setComponentRestrictions({country: 'us'});
```

## Types

```javascript { .api }
/**
 * Google Maps API types used by geocomplete
 * (These are provided by the Google Maps JavaScript API)
 */

// Geocoder result structure
interface google.maps.GeocoderResult {
  address_components: google.maps.GeocoderAddressComponent[];
  formatted_address: string;
  geometry: google.maps.GeocoderGeometry;
  place_id: string;
  types: string[];
  // Additional properties when Places API is used
  icon?: string;
  name?: string;
  rating?: number;
  url?: string;
  website?: string;
  vicinity?: string;
  international_phone_number?: string;
  formatted_phone_number?: string;
}

interface google.maps.GeocoderGeometry {
  location: google.maps.LatLng;
  location_type: string;
  viewport: google.maps.LatLngBounds;
  bounds?: google.maps.LatLngBounds;
}

interface google.maps.GeocoderAddressComponent {
  long_name: string;
  short_name: string;
  types: string[];
}

// Geocoder status codes
enum google.maps.GeocoderStatus {
  ERROR = "ERROR",
  INVALID_REQUEST = "INVALID_REQUEST",
  OK = "OK",
  OVER_QUERY_LIMIT = "OVER_QUERY_LIMIT",
  REQUEST_DENIED = "REQUEST_DENIED",
  UNKNOWN_ERROR = "UNKNOWN_ERROR",
  ZERO_RESULTS = "ZERO_RESULTS"
}

// Geocoder request structure
interface google.maps.GeocoderRequest {
  address?: string;
  location?: google.maps.LatLng;
  bounds?: google.maps.LatLngBounds;
  componentRestrictions?: google.maps.GeocoderComponentRestrictions;
  region?: string;
  placeId?: string;
}

// Map and Marker types (partial, see Google Maps API docs for complete definitions)
interface google.maps.Map {
  setCenter(latLng: google.maps.LatLng): void;
  setZoom(zoom: number): void;
  getZoom(): number;
  getBounds(): google.maps.LatLngBounds;
  setMapTypeId(mapTypeId: string): void;
  // ... additional methods
}

interface google.maps.Marker {
  setPosition(latLng: google.maps.LatLng): void;
  setAnimation(animation: google.maps.Animation): void;
  setMap(map: google.maps.Map | null): void;
  // ... additional methods
}

interface google.maps.LatLng {
  lat(): number;
  lng(): number;
  toUrlValue(): string;
  // ... additional methods
}

interface google.maps.places.Autocomplete {
  getPlace(): google.maps.places.PlaceResult;
  setBounds(bounds: google.maps.LatLngBounds): void;
  setTypes(types: string[]): void;
  setComponentRestrictions(restrictions: google.maps.places.ComponentRestrictions): void;
  bindTo(key: string, target: google.maps.MVCObject): void;
  unbindAll(): void;
  // ... additional methods
}
```

## Error Handling

```javascript
// Handle geocoding errors
$("#address")
  .geocomplete()
  .bind("geocode:error", function(event, status){
    switch(status) {
      case google.maps.GeocoderStatus.ZERO_RESULTS:
        console.error("No results found for this address");
        break;
      case google.maps.GeocoderStatus.OVER_QUERY_LIMIT:
        console.error("Query limit exceeded. Please try again later");
        break;
      case google.maps.GeocoderStatus.REQUEST_DENIED:
        console.error("Geocoding request denied. Check API key permissions");
        break;
      case google.maps.GeocoderStatus.INVALID_REQUEST:
        console.error("Invalid geocoding request");
        break;
      default:
        console.error("Geocoding error:", status);
    }
  });
```

## Browser Compatibility

Geocomplete requires:
- jQuery 1.9.0 or later
- Modern browser with JavaScript enabled
- Internet connection to access Google Maps API
- Google Maps JavaScript API key (recommended for production)

## Important Notes

1. **Google Maps API Key**: While the examples may work without an API key for testing, you should always use an API key in production. Obtain one from the [Google Cloud Console](https://console.cloud.google.com/).

2. **API Usage Limits**: Google Maps API has usage limits and quotas. Monitor your usage to avoid hitting quota limits.

3. **Attribution Requirements**: When using the Places Autocomplete without a map, you must display Google attribution per their terms of service.

4. **Form Submit Prevention**: The plugin automatically prevents form submission when Enter is pressed in the geocomplete input field to avoid accidental form submissions during autocomplete selection.

5. **Memory Management**: When removing geocomplete inputs from the DOM, call the `destroy()` method to clean up event listeners and prevent memory leaks.

6. **Bounds Behavior**: When `bounds: true` is set with a map, the autocomplete results are biased to the visible map area. Without a map, bounds must be explicitly provided as a `google.maps.LatLngBounds` object.
