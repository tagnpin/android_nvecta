# CHANGE LOG

## Version 5.8.5 *(August 10, 2026)*

### Added

* Introduced Global Attributes support to automatically attach common attributes to events.
* Added the new `utm_visit` event to improve visit tracking.
* Added ad campaign-related UTM parameters to cookies.

### Improved

* Enhanced SubscriptionID security by moving its storage from SharedPreferences to a static global variable.
* Standardized app version reporting across API requests by using the app version name instead of the app version code, aligning with the Events API.
* Updated the in-app sticky banner layout by adjusting the position of the close (X) button.

### Removed

* Removed the `utm_sess_change` parameter from Settings API requests to streamline the request payload.


## Version 5.8.4 *(June 23, 2026)*

### Improved

- Optimized location manager operations by moving processing from the main thread to a background thread, improving overall performance.
- Moved database initialization and related operations to a background thread to reduce app startup overhead.
- Updated GIF push CTA styling:
  - Changed the CTA background to transparent.
  - Updated the CTA text color to automatically match the configured accent color.

### Fixed

- Added robust exception handling for location and push notification permission prompts to improve SDK stability.
- Fixed a crash caused by a `null` context during app launch when handling the location permission flow.
- Added additional null and safety checks while parsing query parameters in the `parseDeepLink()` function to prevent unexpected runtime errors.