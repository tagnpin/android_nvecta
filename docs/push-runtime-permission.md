# Notification Runtime Permission

Starting with Android 13 (API Level 33), applications must request notification permission at runtime before sending push notifications.


Official Documentation: <br>
https://www.nvecta.com/docs/notification-runtime-permission


---

NVECTA provides three different ways to handle notification permission requests:

1. Use your own custom permission UI.
2. Use NVECTA's customizable permission prompt.
3. Use the native Android system permission prompt.

## Import Package

**Java**

```java
import com.notifyvisitors.notifyvisitors.NotifyVisitorsApi;
import com.notifyvisitors.notifyvisitors.interfaces.OnPushRuntimePermission;
import com.notifyvisitors.notifyvisitors.permission.NVPopupDesign;
```

**Kotlin**

```kotlin
import com.notifyvisitors.notifyvisitors.NotifyVisitorsApi
import com.notifyvisitors.notifyvisitors.interfaces.OnPushRuntimePermission
import com.notifyvisitors.notifyvisitors.permission.NVPopupDesign
```

---

<br>

# Option 1: Use Your Own Permission UI

If your application already has a custom permission screen or onboarding flow, you can inform the NVECTA SDK whether the user allowed or denied notification permission.

**Java**

```java
NotifyVisitorsApi.getInstance(activityContext).enablePushPermission(isAllowed (boolean));
```

**Kotlin**

```kotlin
NotifyVisitorsApi.getInstance(activityContext).enablePushPermission(isAllowed (boolean))
```

### Parameters

| Parameter | Type | Description                                                                      |
| --------- | ---- | -------------------------------------------------------------------------------- |
| isAllowed | bool | Pass `true` if the user granted notification permission, otherwise pass `false`. |

### Example

**Java**

```java
NotifyVisitorsApi.getInstance(activityContext).enablePushPermission(true);
```

**Kotlin**

```kotlin
NotifyVisitorsApi.getInstance(activityContext).enablePushPermission(true)
```

### When to Use

* You already have a custom-designed permission screen.
* You want full control over the permission request flow.
* You want to show an educational screen before triggering the Android permission dialog.

---

<br>

# Option 2: Use NVECTA's Custom Permission Prompt

NVECTA provides pre-built permission prompt templates that can be customized to match your application's branding.

The prompt contains:

* Title
* Description
* Allow Button
* Deny Button

## Configure Prompt Design

**Java**

```java
NVPopupDesign design = new NVPopupDesign();

design.setTitle("text");
design.setTitleTextColor("color_in_hexcode_format");
design.setDescription("text");
design.setDescriptionTextColor("color_in_hexcode_format");
design.setBackgroundColor("color_in_hexcode_format");
design.setNumberOfSessions(count_as_integer);
design.setResumeInDays(days_as_integer);
design.setNumberOfTimesPerSession(count_as_integer);
design.setTemplateID(integer);

design.setButtonOneBorderColor("color_in_hexcode_format");
design.setButtonOneBorderRadius(radius_as_integer);
design.setButtonOneText("text");
design.setButtonOneTextColor("color_in_hexcode_format");
design.setButtonOneBackgroundColor("color_in_hexcode_format");

design.setButtonTwoText("text");
design.setButtonTwoTextColor("color_in_hexcode_format");
design.setButtonTwoBackgroundColor("color_in_hexcode_format");
design.setButtonTwoBorderColor("color_in_hexcode_format");
design.setButtonTwoBorderRadius(radius_as_integer);

NotifyVisitorsApi.getInstance(activityContext).activatePushPermissionPopup(design, new OnPushRuntimePermission() {
   @Override
   public void getPopupInfo(JSONObject result) {
      //do your task here
   }
});
```

**Kotlin**

```kotlin
val design = NVPopupDesign()
design.setTitle("text")
design.setTitleTextColor("color_in_hexcode_format")
design.setDescription("text")
design.setDescriptionTextColor("color_in_hexcode_format")
design.setBackgroundColor("color_in_hexcode_format")
design.setNumberOfSessions(count_as_integer)
design.setResumeInDays(days_as_integer);
design.setNumberOfTimesPerSession(count_as_integer)
design.setTemplateID(integer)

design.setButtonOneBorderColor("color_in_hexcode_format")
design.setButtonOneBorderRadius(radius_as_integer)
design.setButtonOneText("text")
design.setButtonOneTextColor("color_in_hexcode_format")
design.setButtonOneBackgroundColor("color_in_hexcode_format")

design.setButtonTwoText("text")
design.setButtonTwoTextColor("color_in_hexcode_format")
design.setButtonTwoBackgroundColor("color_in_hexcode_format")
design.setButtonTwoBorderColor("color_in_hexcode_format")
design.setButtonTwoBorderRadius(radius_as_integer)

NotifyVisitorsApi.getInstance(this).activatePushPermissionPopup(design, object : OnPushRuntimePermission {
  override fun getPopupInfo(it: JSONObject) {
    //do your task here
  }
})
```

## Session Control Parameters

### numberOfSessions

Determines how many app sessions the prompt should be shown after a user dismisses it.

A new session is created when:

* The app is launched for the first time.
* The user returns after 30 minutes of inactivity.

Example:

```dart
design.numberOfSessions = "3";
```

The prompt will appear for the next 3 sessions if the user continues to dismiss it.

---

### resumeInDays

Controls when the prompt should reappear after all configured sessions are exhausted.

Example:

```dart
design.setResumeInDays = "10";
```

The prompt will remain hidden for 10 days and become eligible to show again from the 11th day.

---

### setNumberOfTimesPerSession

Controls the maximum number of times the prompt can be shown within a single session.

```dart
design.setNumberOfTimesPerSession = "6";
```

---

<br>

# Option 3: Use Native Android Permission Prompt

If you prefer the standard Android system permission dialog, use:

**Java**

```java
NotifyVisitorsApi.getInstance(activityContext).nativePushPermissionPrompt(new OnPushRuntimePermission() {
   @Override
   public void getPopupInfo(JSONObject result) {
       //do your work here
   }
});
```

**Kotlin**

```kotlin
NotifyVisitorsApi.getInstance(activityContext).nativePushPermissionPrompt(object : OnPushRuntimePermission {
  override fun getPopupInfo (result: JSONObject) {
    //do your work here
  }
})
```

### Sample Response

```json
{
  "status": "success",
  "message": "Popup launched. User granted permission."
}
```

## Important

>Android generally allows notification permission requests only a limited number of times.

>If the user repeatedly denies permission, the application may need to redirect them to the device's notification settings page to enable notifications manually.

---

<br>

# Callback Responses

The callback from both Option 2 and Option 3 can return the following responses.

| Status  | Message                                                                        |
| ------- | ------------------------------------------------------------------------------ |
| Success | Push permission is already active on this device.                              |
| Success | Popup launched. User granted permission.                                       |
| Success | Push Notification Settings is enabled by default on Android versions below 13. |
| Success | Push Notification Settings is ON.                                              |
| Fail    | Popup launched. User denied NVECTA's custom permission prompt.                 |
| Fail    | Popup launched. User denied permission.                                        |
| Error   | Something went wrong with error `<error>`                                      |

---

<br>

# Recommended Approach

For the best user experience:

1. Show an educational screen explaining the benefits of notifications.
2. Display NVECTA's custom permission prompt (Option 2).
3. Trigger the Android system permission dialog.
4. If permission is denied, guide users to app notification settings.

This approach generally results in higher notification opt-in rates because users understand why the permission is being requested before seeing the system prompt.

<br>

# Related Documentation

Continue exploring other SDK features:

- 🔔 [Push Notifications](/docs/push-integration.md)
- 🔔 [Push Notification Channels](/docs/push-notification-channels.md)
- 📥 [Notification Center](/docs/notification-center-integration.md)
- 📊 [Track Events](/docs/event-tracking-integration.md)
- 👤 [Track Users](/docs/user-tracking-integration.md)
- 🎯 [In-App Notifications](/docs/inapp-integration.md)
- 🎯 [In-App Native Nudges](/docs/inapp-nudges.md)
- 🔗 [Deep Links](/docs/deep-link-handling.md)