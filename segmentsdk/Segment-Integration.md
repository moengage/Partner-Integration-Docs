# Getting Started
Once the Segment library is integrated with your app, toggle MoEngage on in your Segment 
integrations. These new settings will take up to an hour to propagate to all of your existing 
users. For new users it’ll be instantaneous!
Segment-MoEngage Integration is a bundled integration, requires client side integration.

Follow the below steps for integration

## Setup MoEngage in Segment Dashboard:

To setup MoEngage do the following : 
  1. First get your key([AppID](http://app.moengage.com/v3/#/settings/0/0)) from MoEngage dashboard. 
  2. Go to **Segment dashboard**, go to **Integrations** and select **MoEngage**. 
  3. Enable MoEngage Integration.
  4. Go to MoEngage Settings and enter the MoEngage AppID, obtained in **Step1**.
  5. Save the changes.
  6. Make sure the `Connection Mode` is set to `Device Mode`. This is required in order to make use
      of features like push notification and in-app feature of MoEngage SDK.
  
These new settings will take up to an hour to propagate to all of your existing users. For new 
users it’ll be instantaneous! Segment-MoEngage Integration is a bundled integration, requires client side integration.

![Settings](/segmentsdk/segment_settings.png)

## iOS


To get started with MoEngage on iOS, first integrate your app with the [MoEngage-Segment-iOS](https://github.com/moengage/MoEngage-Segment-iOS) library. MoEngage can be integrated via Segment using [CocoaPods](http://cocoapods.org). 

  * Initialise pod with pod init command, this will create a podfile for your project.
  * Update your podfile by adding pod '**Segment-MoEngage**' as shown below:

  ```ruby
  use_frameworks!
  pod 'Segment-MoEngage’
  ```

   * Update the pod. 
   
    pod update

### Setup Segment SDK:

Now head to the App Delegate file, and setup the Segment SDK by adding `SEGMoEngageIntegrationFactory` instance to the `SEGAnalyticsConfiguration` as shown below:

 ```
 #import <SEGMoEngageIntegrationFactory.h> // This line is key for MoEngage integration
 #import <SEGAnalytics.h>

 - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
    // Add your configuration key from Segment
    SEGAnalyticsConfiguration *config = [SEGAnalyticsConfiguration configurationWithWriteKey:@"configuration key"];
    
    // Add MoEngageIntegrationFactory. Without this data will not come to MoEngage.
    [config use:[SEGMoEngageIntegrationFactory instance]];
    [SEGAnalytics setupWithConfiguration:config];
  }
  ```

### Tracking User Attribute

User attributes are specific traits of a user, like email, username, mobile, gender etc. **identify** lets you tie a user to their actions and record traits about them. It includes a unique User ID and any optional traits you know about them.

 ```[[SEGAnalytics sharedAnalytics] identify:@"a user's id" traits:@{ @"email": @"a user's email address" }];```

For more info refer to this [link](https://segment.com/docs/sources/mobile/ios/#identify).

### Tracking Events

Event tracking is used to track user behaviour in an app. **track** lets you record the actions your users perform. Every action triggers i.e,“event”, which can also have associated attributes.

 ```[[SEGAnalytics sharedAnalytics] track:@"Item Purchased" properties:@{ @"item": @"Sword of Heracles"}];```

That's all you need for tracking data. For more info refer this [link](https://segment.com/docs/sources/mobile/ios/#track).

### Reset Users

The *reset* method clears the SDK’s internal stores for the current user. This is useful for apps where users can log in and out with different identities over time.

 ```[[SEGAnalytics sharedAnalytics] reset];```

For more info refer to this [link](https://segment.com/docs/sources/mobile/ios/#reset).

### Install / Update Differentiation 

Since you might integrate us when your app is already on the App Store, we would need to know whether your app update would be an actual UPDATE or an INSTALL.
To differentiate between those, use one of the method below:

 ```
 //For new Install call following
 [[MoEngage sharedInstance]appStatus:INSTALL];

 //For an app update call following
 [[MoEngage sharedInstance]appStatus:UPDATE];
 ```

For more info on this refer following [link](https://docs.moengage.com/docs/installupdate-differentiation).

### Data Redirection

We support data redirection to our EU servers in our SDK. This can be done by calling `redirectDataToRegion:` method to redirect the data to `MOE_REGION_EU` region defined in DataRedirectionRegion Enumerator. Use the method as shown below:

```
 [MoEngage redirectDataToRegion:MOE_REGION_EU];
```

**IMPORTANT** :
To sign up to our EU environment go to the following [URL](https://app-eu.moengage.com).

### Using features provided in MoEngage SDK

Along with tracking your user's activities, MoEngage iOS SDK also provides additional features which can be used for more effective user engagement:

#### Push Notifications:
Push Notifications are a great way to keep your users engaged and informed about your app. You have following options while implementing push notifications in your app:

**Segment Push Implementation:**

1.Follow the directions to register for push using Segment SDK in this [link](https://segment.com/docs/libraries/ios/#how-do-i-use-push-notifications-).

2.In your application’s application:didReceiveRemoteNotification: method, add the following:

 ```[[SEGAnalytics sharedAnalytics] receivedRemoteNotification:userInfo];```

3.If you integrated the application:didReceiveRemoteNotification:fetchCompletionHandler: in your app, add the following to that method:
 
 ```[[SEGAnalytics sharedAnalytics] receivedRemoteNotification:userInfo];```

4.If you implemented handleActionWithIdentifier:forRemoteNotification:, add the following to that method:

 ```[[SEGAnalytics sharedAnalytics] handleActionWithIdentifier:identifier forRemoteNotification:userInfo];```
 
**MoEngage Push Implementation:**
 Follow this link to implement Push Notification in your mobile app using MoEngage SDK : 
 [**Push Notifications**](https://docs.moengage.com/docs/push-notifications)


#### In-App Messaging:

In-App Messaging are custom views which you can send to a segment of users to show custom messages or give new offers or take to some specific pages. Follow the link to know more about  inApp Messaging and how to implement it in your application: 
[**InApp NATIV**](https://docs.moengage.com/docs/in-app-nativ)


#### GDPR Compliance:
MoEngage SDK is GDPR compliant, follow the doc in this [link](https://docs.moengage.com/docs/gdpr-compliance-1) to know how to use opt-outs for different features. 

### Segment Docs:
For more info on using **Segment for iOS** refer to [**Developer Docs**](https://segment.com/docs/sources/mobile/ios/) provided by Segment.


## Android

![Downlaod](https://api.bintray.com/packages/moengage/android-sdk/moengage-segment-integration/images/download.svg)


To get up and running with MoEngage on Android, there a couple of steps we will walk you through.

To enable its full functionality (like Push Notifications, InApp Messaging), there are still a couple of steps that you have to take care of in your Android app.

#### Adding MoEngage Dependency:

Along with the segment, dependency add the below dependency in your build.gradle file.

```groovy
 implementation("com.moengage:moengage-segment-integration:$sdkVersion") {
        transitive = true
    }
```
where `$sdkVersion` should be replaced by the latest version of the MoEngage SDK.

MoEngage SDK depends on the below Jetpack libraries provided by Google for its functioning, make you add them if not
 done already.
 
```groovy
    implementation("androidx.core:core:1.3.1")
    implementation("androidx.appcompat:appcompat:1.2.0")
    implementation("androidx.lifecycle:lifecycle-process:2.2.0")
```
Refer to the [SDK Configuration](https://docs.moengage.com/docs/android-sdk-configuration) documentation to know more about the build config and other libraries used by the SDK.
 
#### Register MoEngage with Segment SDK:

After adding the dependency, you must register the integration with Segment SDK. To do this, import the MoEngage
 integration:

```java
import com.segment.analytics.android.integrations.moengage.MoEngageIntegration;
```

Add the following line:

```java
Analytics analytics = new Analytics.Builder(this, "write_key")
                .use(MoEngageIntegration.FACTORY)
                .build();
```


#### How to Initialise MoEngage SDK: 
Get APP ID from the Settings Page `Dashboard --> Settings --> App --> General` and initialize the MoEngage SDK in the `Application` class's `onCreate()`
***Note: It is recommended that you initialize the SDK on the main thread inside `onCreate()` and not create a worker thread and initialize the SDK on that thread.*** 

```java
// this is the instance of the application class and "XXXXXXXXXXX" is the APP ID from the dashboard.
MoEngage moEngage = new MoEngage.Builder(this, "XXXXXXXXXXX")
            .enableSegmentIntegration()
            .build();
MoEngage.initialise(moEngage);
```

#### Install/Update Differentiation
This is solely required for migration to the MoEngage Platform. We need your help to tell the SDK whether the user is
 a new user for your application, or an existing user who has updated to the latest version.

If the user was already using your application and has just updated to a new version which has MoEngage SDK it is an updated call the below API

```java
MoEHelper.getInstance(getApplicationContext()).setAppStatus(AppStatus.UPDATE);
```

In case it is a fresh install call the below API

```java
MoEHelper.getInstance(getApplicationContext()).setAppStatus(AppStatus.INSTALL);
```

#### How To - Push Notifications:
Copy the Server Key from the FCM console and add it to the MoEngage Dashboard(Not sure where to find the Server Key refer to [Getting FCM Server Key](https://docs.moengage.com/docs/getting-fcmgcm-server-key)). To upload it, go to the Settings Page `Dashboard --> Settings --> Channel --> Push --> Mobile Push --> Android` and add the Server Key and package name.
**Please make sure you add the keys both in Test and Live environment.**

##### Adding meta information for push notification

To show push notifications some metadata regarding the notification is required where the small icon and large icon drawables being the mandatory ones.
Refer to the [NotificationConfig](https://moengage.github.io/android-api-reference/core/core/com.moengage.core.config/-notification-config/index.html) API reference for all the possible options.

Use the `configureNotificationMetaData()` to pass on the configuration to the SDK.

```java
MoEngage moEngage =
        new MoEngage.Builder(this, "XXXXXXXXXX")
            .configureNotificationMetaData(new NotificationConfig(R.drawable.small_icon, R.drawable.large_icon, R.color.notiColor, "sound", true, true, true))
            .enableSegmentIntegration()
            .build();
MoEngage.initialise(moEngage);
```

##### Configuring Firebase Cloud Messaging

For showing Push notifications there are 2 important things 

1. Registration for Push, i.e. generating push token.
2. Receiving the Push payload from Firebase Cloud Messaging(FCM) service and showing the notification on the device. 

##### Push Registration and Receiving handled by App

By default, MoEngage SDK attempts to register for push token, since your application is handling push you need to opt-out of SDK's token registration.

###### How to opt-out of MoEngage Registration?

To opt-out of MoEngage's token registration mechanism disable token registration while configuring FCM in the `MoEngage.Builder` as shown below

```java
MoEngage moEngage = new MoEngage.Builder(this, "XXXXXXXXXX")
            .configureNotificationMetaData(new NotificationConfig(R.drawable.small_icon, R.drawable.large_icon, R.color.notiColor, "sound", true, true, true))
            .configureFcm(FcmConfig(false))
            .enableSegmentIntegration()
            .build();
MoEngage.initialise(moEngage);
```

###### Pass the Push Token To MoEngage SDK
The Application would need to pass the Push Token received from FCM to the MoEngage SDK for the MoEngage platform to send out push notifications to the device.
Use the below API to pass the push token to the MoEngage SDK.

```java
MoEFireBaseHelper.getInstance().passPushToken(getApplicationContext(), token);
```
*Note:* Please make sure token is passed to MoEngage SDK whenever push token is refreshed and on application update. Passing token on application update is important for migration to the MoEngage Platform.

###### Passing the Push payload to the MoEngage SDK
To pass the push payload to the MoEngage SDK call the MoEngage API from the `onMessageReceived()` from the Firebase receiver.
Before passing the payload to the MoEngage SDK you should check if the payload is from the MoEngage platform using the helper API provided by the SDK.

```java
if (MoEPushHelper.getInstance().isFromMoEngagePlatform(remoteMessage.getData())) {
  MoEFireBaseHelper.Companion.getInstance().passPushPayload(getApplicationContext(), remoteMessage.getData());
}else{
  // your app's business logic to show notification
}
```

##### Push Registration and Receiving handled by SDK
Add the below code in your manifest file.

```xml
<service android:name="com.moengage.firebase.MoEFireBaseMessagingService">
 	<intent-filter>
		<action android:name="com.google.firebase.MESSAGING_EVENT" />
 	</intent-filter>
</service>
```
When MoEngage SDK handles push registration it optionally provides a callback to the application whenever a new token is registered or token is refreshed. 
Application can get this callback by implementing `FirebaseEventListener` and registering for a callback in the Application class' `onCreate()` using `MoEFireBaseHelper.Companion.getInstance().setEventListener()` 

When MoEngage SDK handles push registration it optionally provides a callback to the Application whenever a new token is registered or token is refreshed.
An application can get this callback by implementing `FirebaseEventListener` and registering for a callback in the Application class' `onCreate()` using `MoEFireBaseHelper.getInstance().addEventListener()`

##### 4. Declaring & configuring Rich Landing Activity: 
Add the following snippet and replace `[PARENT_ACTIVITY_NAME]` with the name of the parent 
 activity; `[ACTIVITY_NAME]` with the activity name which should be the parent of the Rich Landing Page

 ```xml
 <activity
    android:name="com.moe.pushlibrary.activities.MoEActivity"
    android:label="[ACTIVITY_NAME]"
    android:parentActivityName="[PARENT_ACTIVITY_NAME]" >
 </activity>
 ```

You are now all set up to receive push notifications from MoEngage. For more information on features provided in MoEngage Android SDK refer to the following links: 

 * [Push Notifications](http://docs.moengage.com/docs/push-configuration)

 * [Geofence](https://docs.moengage.com/docs/android-geofence)
 
 * [In-App messaging](https://docs.moengage.com/docs/android-in-app-nativ)
 
 * [Notification Center](https://docs.moengage.com/docs/android-notification-center)
 
 * [Advanced Configuration](https://docs.moengage.com/docs/android-advanced-integration)
 
 * [API Reference](https://moengage.github.io/android-api-reference/-modules.html)
 
 * [Compliance](https://docs.moengage.com/docs/android-compliance)
 

#### Identify
Use [Identify](https://segment.com/docs/sources/mobile/android/#identify) to track user-specific attributes. It is equivalent to tracking [user attributes](http://docs.moengage.com/docs/identifying-user) on MoEngage. MoEngage supports traits supported by Segment as well as custom traits. If you set traits.id, we set that as the Unique ID for that user.

#### Track
Use [track](https://segment.com/docs/sources/mobile/android/#track) to track events and user behavior in your app.
This will send the event to MoEngage with the associated properties. Tracking events is essential and will help you create segments for engaging users.

#### Reset
If your app supports the ability for a user to logout and login with a new identity, then you’ll need to call reset for the Analytics client.

#### Sample Implementation

Refer to [this](https://github.com/moengage/moengage-segment-integration) github repository for sample implementation

## Web 

MoEngage WebSDK offers the capability to send push notifications to Google Chrome, Opera and Firefox browsers. There are some additional steps apart from integrating Segment's `analytics.js`.


### Integration

#### 1. Setup your MoEngage Web SDK settings at MoEngage Dashboard
Please setup the [web settings](https://app.moengage.com/v3/#/settings/push/web) on the MoEngage dashboard in order to start using MoEngage <> Segment integration. 

If you have selected `HTTPS` mode of integration in the settings, there are some additional steps to be taken

#### 2 Set up for HTTPS websites
#### 2.a Download the required files (HTTPS only)
For HTTPS Web Push to work, you need to host two files in the `root` directory of your web server. These two files will be available for you to download at the [web settings page](https://app.moengage.com/v3/#/settings/push/web).
* manifest.json
* serviceworker.js

NOTE: Please make sure the name of the serviceworker file is exactly `serviceworker.js`. Please contact MoEngage support at support@moengage.com if you wish to have some other name for the serviceworker file.

#### 2.b Add link to manifest in HTML (HTTPS only)
Add the following line in the <head> tag of your page.

```
<head>
  ...
	<link rel="manifest" href="/manifest.json">
  ...
</head>
```

#### 2.c Use your existing manifest or serviceworker file (HTTPS only)
If you already have these files,

1. Manifest

Add the sender ID you saved on MoEngage dashboard as the `gcm_sender_id`. If you've used `MoEngage Shared Project` while setting up, your sender id is `540868316921`.

Please edit your `manifest.json` as follows:
```
{
  ...
  "gcm_sender_id": "GCM_SENDER_ID",
  ...
}
```
2. Service Worker

Just add the following line to the top of your `serviceworker.js` file
```
importScripts("//cdn.moengage.com/webpush/releases/serviceworker_cdn.min.latest.js?date="+
new Date().getUTCFullYear()+""+new Date().getUTCMonth()+""+new Date().getUTCDate());
```

### Identify
Use [Identify](https://segment.com/docs/sources/website/analytics.js/#identify) to track user specific attributes. It equivalent to [tracking user attributes](https://docs.moengage.com/docs/tracking-web-user-attributes) on MoEngage. MoEngage supports traits supported by Segment as well as custom traits.

### Track
Use [track](https://segment.com/docs/sources/website/analytics.js/#track) to track events and user behaviour in your app. This will send the event to MoEngage with the associated properties. Tracking events is essential and will help you create segments for engaging users.

### Reset
If your website supports the ability for a user to logout and login with a new identity, then you’ll need to call [reset](https://segment.com/docs/sources/website/analytics.js/#reset-logout) method in `analytics.js`.

### Optional
There are some further optional features you can read about here:
* [Configure opt in type](https://docs.moengage.com/docs/configuring-notification-opt-in)
* [Self-handled opt-ins](https://docs.moengage.com/docs/self-handled-opt-ins)
* [SDK callbacks](https://docs.moengage.com/docs/tracking-opt-ins-on-your-own)

### Test Mode and Debugging
While updating the MoEngage settings on the Segment Dashboard, you can enable the logging functionality of the MoEngage SDK to see the SDK logs on the browser console. Just set `Enable Debug Logging` to `On` and the SDK will be loaded in debug mode.

NOTE: When debug mode is enabled, the events and attributes of the users are sent to the `TEST` environment of your MoEngage App.