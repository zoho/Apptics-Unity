Unity plugin for Apptics, wrapper around native iOS and Android SDKs for Apptics. Supports tracking in-app events, screens and sessions for Android and iOS Apps.

#### Prerequisites
- Requires Unity version 2022.3 or above.
- Register your app and download configuration files (apptics-config.json for Android and apptics-confg.plist for iOS)

#### Installation

Apptics Unity plugin uses External Dependency Manager for Unit (EDM4U) to resolve dependencies.

- Download the latest ```AppticsUnity.unitypackage``` from GitHub releases.
- Import it into your Unity Project. 

**Setup for Android Build**

- Resolve Android dependencies. (Assets -> External Dependency Manager -> Android Resolver -> Resolve)
- Add the following snippet in ```launcherTemplate.gradle``` file
```
apply plugin: 'apptics-plugin'

apptics {
    supportEventAsStrings = ["default" : true]
    
    // replace the value with zak from apptics-config.json file.
    zak = ["default": "REPLACE WITH ZAK VALUE FROM apptics-config.json"]
}

```

#### Initializing Apptics

Must be called from main thread, usually in Start function of a MonoBehaviour.
```
using Apptics;

Apptics.init();
```

##### Event Tracking

- Configure custom events in Apptics web console.
- Track the configured events with properties using addEvent method.

```
using Apptics;

// track event
Apptics.addEvent("eventName", "evenGroupName");

// track event with properties
Dictionary<string, string> props = new Dictionary<string, string>();
props.Add("prop_key","value");

Apptics.addEvent("eventName", "eventGroupName", props);
```

#### Screen Tracking

screenDetached must be called for every screenAttached for screens to be logged in Apptics.

```
using Apptics;

// call when screen appears
Apptics.screenAttached("screenName");

// call when screen disappears
Apptics.screenDetached("screenName");
```

#### Track User
- Apptics allows you to associate the tracking data with a user id of an individual using your app. Setting a user id is not mandatory for Apptics to work normally.
- User Id tracking will happen with respect to tracking settings. If the tracking settings is 'Without PII', the data will not be associated with the user.

```
using Apptics;

Apptics.setUser("userId");
```

#### Tracking Settings

Apptics offer 7 tracking states to control usage and crash tracking.
 - UsageAndCrashTrackingWithPII
 - UsageAndCrashTrackingWithoutPII
 - OnlyUsageTrackingWithPII
 - OnlyUsageTrackingWithoutPII
 - OnlyCrashTrackingWithPII
 - OnlyCrashTrackingWithoutPII
 - NoTracking

**UsageAndCrashTrackingWithoutPII is the default state out of the box.**

Usage Tracking:
Usage tracking generally refers to tracking the Events, APIs, Screens, Sessions.

Crash Tracking:
Crash tracking is self-explanatory and refers to the tracking of undhandled exceptions.
PII

The term PII is used to denote the value you set using the Apptics.setUser method.

You can use setTrackingState method to change the tracking state.
```
using Apptics;

// to change the tracking settings to track only crash and associate user id with crash.
Apptics.setTrackingState(TrackingState.OnlyCrashTrackingWithPII);

// to get the current tracking state
Apptics.getTrackingState();
```