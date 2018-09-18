# Getting Started
Once the Segment library is integrated with your app, toggle MoEngage on in your Segment integrations. These new settings will take up to an hour to propogate to all of your existing users. For new users it’ll be instanteneous!
Segment-MoEngage Integration is a bundled integration, requires client side integration.

Follow the below steps for integration

## iOS
To get started with MoEngage on iOS, first integrate your app with the MoEngage [iOS](https://segment.com/docs/sources/mobile/ios/) library. To get the APP ID, [login](http://app.moengage.com/login) to your account, click on `Settings` in the left menu and select `App Settings`, here we show the APP ID of your app. Next, go to `Push Settings`, and select `iOS Settings` tab, upload the pem file for Push Notifications here and enter the password for the same. If you don't have the pem already, follow the steps listed [here](http://docs.moengage.com/docs/apns-certificate-pem-file). Finally, you want to ensure you have configured your app delegate to [enable push notifications](https://segment.com/docs/libraries/ios#how-do-i-use-push-notifications-).

For more information on features provided in MoEngage iOS SDK refer to following links:

 * [Push Notifications](http://docs.moengage.com/docs/push-notifications)
 
 * [iOS 10 Rich Notification](http://docs.moengage.com/docs/ios-10-rich-notifications)
 
 * [In-App messaging](http://docs.moengage.com/docs/in-app-nativ)
 
 * [Notification Center](http://docs.moengage.com/docs/ios-notification-center)
 
 * [Geofence based Push Notifications](http://docs.moengage.com/docs/geofences)


#### Identify
Use [Identify](https://segment.com/docs/libraries/ios/#identify) to track user specific attributes. It equivalent to tracking [user attributes](http://docs.moengage.com/docs/tracking-user-attributes) on MoEngage. MoEngage supports traits supported by Segment as well as custom traits. If you set traits.id, we set that as the Unique ID for that user.

#### Track
Use [track](https://segment.com/docs/libraries/ios/#track) to track events and user behaviour in your app.
This will send the event to MoEngage with the associated properties. Tracking events is essential and will help you create segments for engaging users differently.

#### Reset
If your app supports the ability for a user to logout and login with a new identity, then you’ll need to call reset in your mobile app. Here we will call MoEngage's resetUser implementation to ensure the user information remains consistent.


## Android
To get up and running with MoEngage on Android, there a couple of steps we will walk you through.

To enable its full functionality (like Push Notifications, InApp Messaging, Acquisition Tracking), there are still a couple of steps that you have to take care of in your Android app.

#### Adding MoEngage Dependency:

Along with the segment dependency add the below dependency in your build.gradle file.

 compile('com.moengage:moengage-segment-integration:+') {
        transitive = true
    }

#### How To - Push Notifications:
1. Manifest permissions: Copy the permissions XML below into the `AndroidManifest.xml` and insert your package name into the name fields where it says [YOUR_PACKAGE_NAME/applicationId].

 ```xml
<uses-permission android:name="android.permission.INTERNET" />
<permission android:name="[YOUR_PACKAGE_NAME/applicationId].permission.C2D_MESSAGE" android:protectionLevel="signature" />
<uses-permission android:name="[YOUR_PACKAGE_NAME/applicationId].permission.C2D_MESSAGE" />
<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
<uses-permission android:name="android.permission.GET_ACCOUNTS" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<!-- If you want to use our Geo Fencing feature  -->
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
 ```
2. Declaring Receivers: Add the below components into the `AndroidManifest.xml` and replace `[YOUR_PACKAGE_NAME/applicationId]` with your package name or application id for Push.

**When using Play Services 7.3**

 ```xml
<!-- MOENGAGE RECEIVER FOR RECEIVING GCM BROADCAST MESSAGES -->
<receiver
  android:name="com.moengage.receiver.MoEngagePushReceiver"
  android:permission="com.google.android.c2dm.permission.SEND" >
  <intent-filter>
    <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
    <category android:name="[YOUR_PACKAGE_NAME/applicationId]" />
  </intent-filter>
</receiver>
```
**When using Play Services 7.5 and above**
```xml
<receiver
    android:name="com.google.android.gms.gcm.GcmReceiver"
    android:exported="true"
    android:permission="com.google.android.c2dm.permission.SEND" >
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <category android:name="[YOUR_PACKAGE_NAME/applicationId]" />
    </intent-filter>
</receiver>
<service
    android:name="com.moengage.worker.MoEGCMListenerService"
    android:exported="false" >
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
    </intent-filter>
</service>
<service
    android:name="com.moengage.receiver.MoEInstanceIDListener"
    android:exported="false">
    <intent-filter>
        <action android:name="com.google.android.gms.iid.InstanceID"/>
    </intent-filter>
</service>
```

Add the below components into the `AndroidManifest.xml` for Install Intents
```xml
<!-- MOENGAGE RECEIVER FOR RECEIVING INSTALLATION INTENT -->
<receiver android:name="com.moe.pushlibrary.InstallReceiver" >
  <intent-filter>
    <action android:name="com.android.vending.INSTALL_REFERRER" />
  </intent-filter>
</receiver>
 ```

4. Declaring MoEngage Specific Meta Tags: Add the following `meta tags` into the `AndroidManifest.xml`. Description of each meta tag and its value:
    * `[APP_ID]` : MoEngage App Id which you can find under `AppSettings` on MoEngage [Dashboard](http://app.moengage.com/v3/#/settings/0)
    * `[SENDER_ID]` : The Google Project Number as seen on Google Developer console. This is required for GCM registration.
    * `[NOTIFICATION_ICON]`: The Small Notification icon which needs to be shown. The value must be just the image file name.
    * `[NOTIFICATION_LARGE_ICON]`: The large notification icon which will be used for notifications on Lollipop and above. The value must be just the image file name.
    * `[NOTIFICATION_TYPE]`: Whether its multiple or single. By default it is single but specified `@integer/notification_type_multiple` it will be able to show multiple notifications as one go.
    * `[NOTIFICATION_TONE]`: The Notification Tone which will be used to play when the notification is shown. By Default it is picked up from system settings but setting the sound file name will play that sound. Remember the value has to be just the sound file name.
    
 ```xml
<!-- MANDATORY FIELD: APP ID AS SEEN ON MOENGAGE DASHBOARD APP SETTINGS PAGE -->
<meta-data
    android:name="APP_ID"
    android:value="[MOENGAGE_APP_ID]" />
<!-- MANDATORY FIELD: SENDER ID , i.e. THE PROJECT NUMBER AS MENTIONED ON GOOGLE CLOUD CONSOLE PROJECTS PAGE -->
<meta-data
    android:name="SENDER_ID"
    android:value="id:[SENDER_ID]" />
<!-- MANDATORY FIELD: THE NOTIFICATION SMALL ICON WHICH WILL BE USED TO SET TO NOTIFICATIONS POSTED -->
<meta-data
    android:name="NOTIFICATION_ICON"
    android:value="ic_launcher" />
<!-- MANDATORY FIELD: THE NOTIFICATION LARGE ICON WHICH WILL BE USED TO SET TO NOTIFICATIONS POSTED -->
<meta-data
   android:name="NOTIFICATION_LARGE_ICON"
   android:value="large_icon" />
<!-- OPTIONAL FIELD: THE NOTIFICATION TYPE WHICH WILL BE USED, SINGLE OR MULTIPLE. DEFAULT BEHAVIOR IS SINGLE -->
<meta-data
    android:name="NOTIFICATION_TYPE"
    android:value="@integer/notification_type_multiple" />
<!-- OPTIONAL FIELD: THE NOTIFICATION TONE THAT WILL BE USED. IF NOT SET WILL PLAY THE DEFAULT SOUND -->
<meta-data
    android:name="NOTIFICATION_TONE"
    android:value="tring" />
 ```
5. Declaring & configuring Rich Landing Activity: Add the following snippet and replace `[PARENT_ACTIVITY_NAME]` with the name of the parent activity; `[ACTIVITY_NAME]` with the activity name which should be the parent of the Rich Landing Page

 ```xml
 <activity
    android:name="com.moe.pushlibrary.activities.MoEActivity"
    android:label="[ACTIVITY_NAME]"
    android:parentActivityName="[PARENT_ACTIVITY_AME]" >
    <!-- Parent activity meta-data to support 4.0 and lower -->
    <meta-data
        android:name="android.support.PARENT_ACTIVITY"
        android:value="[PARENT_ACTIVITY_AME]" />
 </activity>
 ```
 
 
#### Install/Update Differentiation
We need your help to tell the SDK whether the user is a new user or an existing user.
You can check for an existing SharedPreferences value and confirm whether its a fresh user or an existing user.

```java
SharedPreferences pref = context.getSharedPreferences(PREF_NAME, Context.MODE_PRIVATE);
if( pref.contains("your_key")){
    helper.setExistingUser(true);
}else{
    helper.setExistingUser(false);
}
```
In the above code "your_key" is something you expect in the shared preference file if the app has been run at least once before. Possible values might be (firstRun, AppVersion, SenderId, Push Token, etc)

This code should be done in your Application class  and should be called only once
If this code is not added INSTALL InApp/Push Campaigns will not work

Now log on to [MoEngage Dashboard](http://app.moengage.com/login) and go to `Push Platform Settings` section. Add the GCM API Key which you have generated from the Google Developer console API Access for GCM Module. Also update the app package name.

You are now all setup to receive push notifications from MoEngage. For more information on features provided in MoEngage Android SDK refer to following links: 

 * [Push Notifications](http://docs.moengage.com/docs/push-configuration)
 
 * [In-App messaging](http://docs.moengage.com/docs/configuring-in-app-nativ)
 
 * [Notification Center](http://docs.moengage.com/docs/notification-center)
 
 * [Geofence based Push Notifications](http://docs.moengage.com/docs/configuring-geofence-pushes)

#### Identify
Use [Identify](https://segment.com/docs/sources/mobile/android/#identify) to track user specific attributes. It equivalent to tracking [user attributes](http://docs.moengage.com/docs/identifying-user) on MoEngage. MoEngage supports traits supported by Segment as well as custom traits. If you set traits.id, we set that as the Unique ID for that user.

#### Track
Use [track](https://segment.com/docs/sources/mobile/android/#track) to track events and user behaviour in your app.
This will send the event to MoEngage with the associated properties. Tracking events is essential and will help you create segments for engaging users.

#### Reset
If your app supports the ability for a user to logout and login with a new identity, then you’ll need to call reset for the Analytics client.

#### Migrating from older version(One time process)
Please refer to [this](http://docs.moengage.com/docs/migrating-to-7xxx) link to migrate from SDK version less than 2.0.00


#### Sample Implementation

Refer to [this](https://github.com/moengage/SegmentDemo) github repository for sample implementation

## APP_ID
You can find the APP_ID key under `App Settings` on the MoEngage [dashboard](http://app.moengage.com/v3/#/settings/0)