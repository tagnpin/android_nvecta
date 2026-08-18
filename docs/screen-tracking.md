# Screen Tracking Guide

## 📱 What is Screen Tracking?

Screen tracking monitors which screens (pages) your users visit in your Android app. Think of it like a visitor counter for each page - it records when users open a screen and how long they stay there.

**Simple Example:**
- User opens your app → Home screen is tracked
- User navigates to Profile → Profile screen is tracked
- User goes back to Home → Home is tracked again

Official Documentation: <br>
https://www.nvecta.com/docs/native-android

## ❓ Why Should You Use Screen Tracking?

### 1. **Understand Your Users**
- See which screens users visit most
- Find out where users spend the most time
- Understand how users navigate through your app

### 2. **Improve Your App**
- Identify slow or buggy screens
- Find screens that users leave quickly (and fix them)
- Make data-driven decisions about new features

### 3. **Track User Goals**
- See how many users complete important actions
- Find where users get stuck or stop using the app
- Measure if your changes actually helped

### 4. **Debug Problems**
- When a user reports a bug, see exactly which screens they visited before it happened
- Understand the sequence of screens leading to a crash


## 🚀 How to Implement Screen Tracking

### Automatic Tracking (Recommended)

Once the NotifyVisitors Android SDK is integrated and initialized, the SDK automatically logs screen views whenever a user transitions between Activities or Fragments.

- No additional code changes are required.
- Each screen transition automatically generates a screen_view event that captures:
  - Screen name
  - App version
  - Other contextual information (device, session, user identifiers, etc.)

This ensures you get a complete picture of user journeys across your app without extra development effort.

### Manual Tracking

In some cases, you may want more control over screen tracking, especially when:

- Your app uses custom navigation frameworks.
- You want to track specific sub-views within a single Activity or Fragment.
- You want to override the automatically logged screen name with a custom identifier.

<br>

> [!NOTE]
> **Automatic screen tracking is enabled by default.**
>
> If you want to manually control all screen tracking events, disable automatic screen tracking by adding the following metadata inside your application's `<application>` tag in `AndroidManifest.xml`:
>
> ```xml
> <meta-data
>     android:name="notifyvisitors_analytics_automatic_screen_tracking_enabled"
>     android:value="false" />
> ```
>
> Once disabled, the SDK will no longer automatically log screen views. You must explicitly call `trackScreen()` whenever you want to record a screen view.

Notice that the value should be false, not true, because you're describing how to stop automatic tracking. If the value is true, automatic tracking remains enabled.

#### The Basic Function

**Java**

```java
NotifyVisitorsApi.getInstance(this).trackScreen("screen_name");
```
**Kotlin**

```kotlin
NotifyVisitorsApi.getInstance(this).trackScreen("screen_name")
```

This one line of code tells the SDK: *"User is now on 'ScreenName'"*


#### Parameters
- "screen_name" → A unique identifier (String) for the screen.
   - Use clear, consistent names for screens (e.g., "HomeScreen", "ProductDetails", "CartScreen", "PaymentConfirmation").
   - This naming consistency ensures reports are meaningful.

## `screen_view` Event Response Callback
You can get event trigger response in the form of JSONObject. It will help you to track whether it is successful or not.

**Java**

```java
NotifyVisitorsApi.getInstance(activityContext).getEventResponse(new OnEventTrackListener() {
     @Override
     public void onResponse(JSONObject jsonObject) {
        //do your task
     }
 });
 ```

 **Kotlin**
```kotlin
NotifyVisitorsApi.getInstance(this).getEventResponse(object : OnEventTrackListener{
   override fun onResponse(data: JSONObject?) {
       //do your task here
   }
})
```

**Callback Response**

```jsonobject
{
  "status": "success",
  "eventName": "screen_view",
  "message": "Event accepted for processing",
  "type": 0,
  "callbackType": "event"
}
```


## ✅ Best Practices

### 1. **Use Meaningful Screen Names**
```dart
// ❌ Bad
NotifyVisitorsApi.getInstance(this).trackScreen('Screen1');

// ✅ Good
NotifyVisitorsApi.getInstance(this).trackScreen('HomePage');
NotifyVisitorsApi.getInstance(this).trackScreen('UserProfile');
NotifyVisitorsApi.getInstance(this).trackScreen('ProductDetail_12345');
```

### 2. **Be Consistent**
- Use the same screen name every time you visit that screen
- Use camelCase or PascalCase consistently

### 3. **Track Important Flows**
Focus on tracking screens that matter:
- Login/Signup flows
- Purchase/Checkout flows
- Onboarding screens
- Error/Help screens

### 4. **Include Context When Needed**
```dart
// Good: Includes item ID for detailed analytics
NotifyVisitorsApi.getInstance(this).trackScreen('ProductDetail_${productId}');
```
<br>

# Related Documentation

Continue exploring other SDK features:

- 📊 [Track Events](/docs/event-tracking-integration.md)
- 👤 [Track Users](/docs/user-tracking-integration.md)
- 🎯 [In-App Notifications](/docs/inapp-integration.md)
- 🎯 [In-App Native Nudges](/docs/inapp-nudges.md)
- 🔔 [Push Notifications](/docs/push-integration.md)
- 🔗 [Deep Links](/docs/deep-link-handling.md)

---