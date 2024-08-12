# Changelog

##[1.5.9]
### Added
- Sandbox mode

##[1.5.8]
### Changed
- Network timeouts

##[1.5.7]
### Fixed
- close miniapp page on multitask mode

##[1.5.6]
### Added
- consent t&c, privacy text
### Changed
- consent button color from hostapp colors

##[1.5.4]
### Changed
- updated Sentry endpoint

##[1.5.3]
### Fixed
- add listeners to minapp opened from miniapp 

##[1.5.2]
### Changed
- system info phone model to hardware codename

##[1.5.1]
### Added
- enableSplash flag to miniapp configs
### Fixed
- fix close miniapp logic

##[1.5.0]
### Added
- Consent Management
### Changed
- Increase miniapp menu button size
- Changed icons

##[1.4.14]
### Added
- fullscreen auth page
- url_suffix
### Changed
- default design for bottomsheet dialogs

##[1.4.13] - 2021-12-09
### Added
- clear cache on reload
- disable forceDarkAllowed

##[1.4.12] - 2021-11-15
### Added
- hide clear cache functionality

##[1.4.11] - 2021-11-10
### Changed
- appboxo logo

##[1.4.10] - 2021-09-21
### Added
- remove the word "miniapp" from menu items
- clear cookies on clearCache

##[1.4.9] - 2021-09-14
### Added
- onUserInteraction method to MiniappLifecycle
- clear localStorage on clearCache

##[1.4.8] - 2021-08-26
### Fixed
- onActivityResult invoking from AppboxoActivity

##[1.4.7] - 2021-08-06
### Added
- permissionsPage flag to config
### Changed
- settings page moved to core

##[1.4.6] - 2021-07-30
### Fixed
- isShopboxo flag moved to MiniappInfo
- crash on login

##[1.4.5] - 2021-07-27
### Added
- onGeolocationPermissionsShowPrompt implementation

##[1.4.4] - 2021-07-19
### Added
- Pull miniapp settings for active miniapp from platform every 30seconds and invalidate cache if it has changed

##[1.4.3] - 2021-06-30
### Added
- custom url loading handler
- isShopboxo flag

##[1.4.2] - 2021-06-18
### Changed
- moves Payment to core module
- renamed BaseMiniapp to Miniapp

##[1.4.1] - 2021-06-15
### Added
- added debug mode flag to Config for debugging webview
- added new auth flow
- added sandbox mode
### Fixed
- saving view states on changing screen orientation

## [1.4.0] - 2021-05-26
### Changed
-  separated to light and full SDKs

### Added
- injecting script into miniapp that will come from platform
- support for android api 16
- proguard rules. --keepnames for all classes
- auto screen orientaion in LightSDK

## [1.3.46] - 2021-05-13
### Fixed
- bug with similar miniapps in list

## [1.3.45] - 2021-05-07
### Added
- hide 'install app' banner for Agoda

## [1.3.44] - 2021-05-07
### Added
- support for android api 19

## [1.3.43] - 2021-05-05
### Added
- "category" field to MiniappData

## [1.3.42] - 2021-04-27
### Added
- getMiniapps() method (get approved miniapps)

## [1.3.41] - 2021-04-21
### Added
- sets windowSoftInputMode "adjustResize"

## [1.3.40] - 2021-04-12
### Added
- core version

## [1.3.39] - 2021-03-16
### Added
- support multipleWindows mode for auth with socials
- switcher for appboxo branding, disable whitelisting url flags

## [1.3.38] - 2021-03-02
### Added
- hash sum to analytics tracking request data

### Fixed
- disabled "about:blank"
- get browser_fallback_url from uri with scheme intent://

## [1.3.37] - 2021-02-11
### Changed
- removed params from getMiniapp method
- added new methods to Miniapp to setting auth payload, additional user data, custom data

## [1.3.36] - 2021-01-29
### Fixed
- use "browser_fallback_url" from intent

## [1.3.35] - 2021-01-27
### Fixed
- add extra url params on first launch

## [1.3.34] - 2021-01-25
### Fixed
- json response with JSONObject
- crash on an incorrect app_id with '/' symbol

## [1.3.33] - 2020-12-23
### Added
- new method removeAllListeners()

### Fixed
- remove all listeners on close miniapp
- passing all listeners when miniapp opens another miniapp

## [1.3.32] - 2020-12-07
### Added
- url change listener
- option to add third action menu
- extra query parameters to miniapp URL   

## [1.3.31] - 2020-11-17
### Added
- payment method

## [1.3.30] - 2020-09-28
### Added
- http error handler

### Fixed
- fixed launch analytics

## [1.3.29] - 2020-09-15
### Added
- closing all miniapps on logout
- renamed getMiniApp() to getMiniapp(), class MiniApp to Miniapp
- dark/light support

## [1.3.28] - 2020-09-09
### Changed
- changed text inside auth permission dialog 

## [1.3.27] - 2020-09-08
### Added
- single/multi-window support 

## [1.3.26] - 2020-09-04
### Fixed
- fixed initial action buttons theme
- fixed bug on click action buttons 

## [1.3.25] - 2020-08-26
### Fixed
- fixed clear cache

## [1.3.24] - 2020-08-26
### Fixed
- fixed action buttons position 
- fixed action buttons theme

## [1.3.23] - 2020-08-26
### Fixed
- fixed reload miniapp

## [1.3.22] - 2020-08-25
### Added
- floating action buttons

## [1.3.21] - 2020-08-24
### Changed
- removed internal modifier from MiniApp 'appId' 
### Fixed
- miniapp error layout
- crash on load miniappsettings
- webView sound lags when app going background
- removed platform token from miniapp data

## [1.3.20] - 2020-08-21 
### Added
- additional user fields for auth in miniapps
### Changed
- data field in miniapp from string to map
### Fixed
- don't show the login dialog again if it is already opened

## [1.3.19] - 2020-08-07
### Added
- action button's style from miniapp settings

## [1.3.18] - 2020-07-30
### Added
- miniapp lifecycle listener 

## [1.3.17] - 2020-07-24
### Changed
- moved login action from jssdk
- moved transaction tracking from jssdk

## [1.3.16] - 2020-07-07
### Added
- systemUiMode(light, dark) to system info
- language to system info
- error message when there is no integration
- Appboxo.hideAllMiniApps() method to minimizing all miniapp screens

## [1.3.15] - 2020-07-01
### Added
- AppboxoActivity.doOnActivityResult(...) method
 
## [1.3.14] - 2020-06-24
### Added
- miniapp settings caching
- pass "white_label" field to miniapp settings
- file chooser support
- new methods Appboxo.getMiniApp(...)  with params (authPayload - for auth, data - optional custom data to open miniapp).   
### Fixed
- fixed bug on reload & clear cache
### Deprecated
- Appboxo.createMiniApp(appId, payload)

## [1.3.11] - 2020-06-11
### Added
- custom url schemes handler
### Fixed
- miniapp close animation
- fixed webview error handler
- fixed miniappSettings
   
## [1.3.10] - 2020-05-29
### Added
- MiniApp config class with colors
- fade in to top effect when miniapps are switched
- onRestore method
### Changed
- required fields from miniapp settings

## [1.3.9] - 2020-05-22
### Added
- Miniapp settings
- Compass
- Instant miniapp reopening
- Background color of the window
### Changed
- AppBoxoWebAppShowImages result callback return type
- Render NavBar only when miniapp is loaded

## [1.3.7] - 2020-05-15
### Added
- Bottom context menu
- Status bar based on values from miniapp settings
- Accelerometer
- Gyroscope

## [1.3.5] - 2020-05-08
### Added
- Landscape mode
- Open location
- Choose location
### Changed
- Haptic Feedback duration
### Fixed
- NavigationBar title padding without icon
