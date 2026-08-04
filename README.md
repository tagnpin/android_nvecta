![NVECTA logo; Formerly NotifyVisitors](https://i.ibb.co/MybKMHDm/NVECTA-000.jpg)

# NVECTA Native Android SDK

[![Static Badge](https://img.shields.io/badge/Central_Maven-5.8.4-blue?logo=android)](https://central.sonatype.com/artifact/com.notifyvisitors.notifyvisitors/notifyvisitors/overview) ![Formerly](https://img.shields.io/badge/Formerly-NotifyVisitors-blue)

<br>

## 🚀 Quick Introduction

The NVECTA Android SDK helps you integrate powerful customer engagement and analytics features into your Android mobile applications. With NVECTA, you can track user activity, send personalized notifications, and improve user engagement through real-time insights and communication tools.

To learn more, visit our [website](https://www.nvecta.com/) and explore the [documentation](https://www.nvecta.com/docs/getting-started-3) for installation and setup guidance.

Ready to get started? [Sign up here](https://console.notifyvisitors.com/console/account/login) to create your account.

<br>

## Requirements

- Android API 23+
- AndroidX
- Kotlin or Java
- Android Studio Hedgehog or newer

<br>

## 📦 Integrate NVECTA into Your Android App

#### Step1- To integrate the NVECTA Android SDK into your Android application, add the SDK dependency to your app's `build.gradle` file:

```gradle
dependencies {
    implementation 'com.notifyvisitors.notifyvisitors:notifyvisitors:v5.8.4'
}
```

#### Step2- Add the following repo to your project's `build.gradle` file:

```gradle
repositories {
   google()
   mavenCentral()
}
```

#### Step 3- Manifest Permission

Add the following permission inside **AndroidManifest.xml**.

```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
```

#### Step 4- Initialize the SDK
The SDK **must be initialized inside your Application class**.

> **Important**
>
> Initialize the SDK **only once** from your `Application` class.
>
> Do **not** initialize it from an `Activity`, `Fragment`, or lazy-loaded component.

Create an Application class if you don't already have one.

**Java**

```java
import android.app.Application;
import com.notifyvisitors.notifyvisitors.NotifyVisitorsApplication;

public class MyApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();

        NotifyVisitorsApplication.register(this, "YOUR_NVECTA_BRAND_ID", "YOUR_NVECTA_BRAND_SECRET_KEY");
    }
}
```

**Kotlin**

```kotlin
import android.app.Application
import com.notifyvisitors.notifyvisitors.NotifyVisitorsApplication

class MyApplication : Application() {

    override fun onCreate() {
        super.onCreate()

        NotifyVisitorsApplication.register(this, "YOUR_NVECTA_BRAND_ID", "YOUR_NVECTA_BRAND_SECRET_KEY")
    }
}
```

#### Step 5- Register Your Application Class

```xml
<application
    android:name=".MyApplication"
    ...
/>
```

The SDK is now initialized and ready to use.

<br>

## 📊 What's Next? SDK Features & Guides

Explore the following guides to learn how to use key NVECTA SDK features in your Android application.

### 🎯 Tracking Screens

Track user screen interactions to better understand user app behavior and engagement within your application.

➡️ [View Screen Tracking Documentation](/docs/screen-tracking.md)

### 🔔 Push Notifications

Configure and send push notifications to re-engage users with real-time updates and personalized communication.

**!IMPORTANT**

> To receive push notifications on Android 12+ devices, you must configure the runtime push notification permission prompt in your application. Please refer to the [Android technical](/docs/push-integration.md) documentation to complete the setup correctly.

### 🎯 Tracking Events

Track user interactions and custom events to better understand user behavior and engagement within your application.

➡️ [View Event Tracking Documentation](/docs/event-tracking-integration.md)

### 👤 Tracking Users

Identify users, manage user profiles, and associate user activity for personalized engagement and analytics.

➡️ [View User Tracking Documentation](/docs/user-tracking-integration.md)

### 💬 In-App Notifications & Nudges

Display targeted in-app messages and campaigns to engage users while they are actively using the application.

➡️ [View In-App Notification Documentation](/docs/inapp-integration.md)

### 🎯 In-App Nudges

Guide users with contextual nudges such as embedded banners, cards, tooltips, and other native UI elements to improve engagement and conversions.

➡️ [View In-App Nudges Documentation](/docs/inapp-nudges.md)

### 📥 Notification Center

Manage and display user notifications within a centralized in-app notification center experience.

➡️ [View Notification Center Documentation](/docs/notification-center-integration.md)

<br>

## 🎯 Sample Integration

The [demo application](/docs/running-sample-app-in-local.md) provides a practical implementation example to help you quickly integrate and test the NVECTA Flutter SDK.

<br>

## 🆕 Changelog

Refer to the NVECTA Android SDK [Change Log](CHANGELOG.md).

<br>

## ❓Questions

Need help? Contact the NVECTA support team directly from the NVECTA Dashboard for assistance with integration, configuration, or troubleshooting.
