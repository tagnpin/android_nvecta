# In-App Notifications Guide

Display targeted In-App Notifications to engage users while they are actively using your application.

Use this guide to display notifications, configure targeting, and handle user interaction callbacks.

Official Documentation:  
https://www.nvecta.com/docs/android-in-app-messaging

---

<br>

# What are In-App Notifications?

In-App Notifications are messages displayed directly inside the application while the user is actively using it.

Unlike push notifications, these messages only appear when the application is open.

They can be used for:

- Promotional offers
- Discounts
- Product recommendations
- User onboarding
- Feature announcements
- Surveys
- Subscription reminders
- Engagement campaigns

---

<br>

# Basic In-App Flow

```text
Application Starts
       ↓
SDK Syncs Campaigns
       ↓
show() Called
       ↓
Campaign Rules Evaluated
       ↓
Matching Notification Displayed
       ↓
User Interaction Callback
```

---

<br>

# Import Package

**Java**
```java
import com.notifyvisitors.notifyvisitors.NotifyVisitorsApi;
```
**Kotlin**
```kotlin
import com.notifyvisitors.notifyvisitors.NotifyVisitorsApi
```

---

<br>

# Display In-App Notifications

The SDK provides the `show()` method to evaluate and display In-App Notifications based on configured targeting rules.

---

## Function

**Java**

```java
NotifyVisitorsApi.getInstance(activityContext).show(dynamicTokens, customObject, fragmentName, new OnInAppTriggerListener() {
    @Override
     public void onDisplay(JSONObject response) {
             //do your task here
     }
});
```

**Kotlin**

```java
NotifyVisitorsApi.getInstance(activityContext).show(dynamicTokens, customObject, fragmentName, object : OnInAppTriggerListener {
       override fun onDisplay(response: JSONObject) {
           //do your task here
       }
})
```

---

<br>

## Parameters

| Parameter | Type | Description |
|------------|------------|-------------|
| tokens | JSON Object | Custom values used for campaign targeting |
| rules | JSON Object | Used for personalized content in Notification messages in real time |
| fragmentName | String | Name of the current fragment when using multiple fragments |
| callback | Function | Receives the display result |

---

<br>

# Recommended Usage

Call `show()` once per screen/activity or whenever a screen becomes visible (for example, in `onResume()` or after navigating to a new Fragment).

Example:

**Java**

```java
JSONObject customObj = new JSONObject();
try {
    customObj.put("test","abc");
    customObj.put("PAGE_ID", "DASHBOARD");
} catch (JSONException e) {
    e.printStackTrace();
}

JSONObject tokens = new JSONObject();
try {
    tokens.put("Budget", "CAD $300,000 to $600,000");
    tokens.put("ProjectName", "| New Development | Kings Landing Condos |");
} catch (JSONException e) {
    e.printStackTrace();
}

NotifyVisitorsApi.getInstance(activityContext).show(tokens, customObj, "thirdFragment", new OnInAppTriggerListener() {
    @Override
     public void onDisplay(JSONObject response) {
             //do your task here
     }
});
```

**Kotlin**

```java
val customObj = JSONObject()
try {
   customObj.put("test","abc")
   customObj.put("PAGE_ID", "DASHBOARD")
} catch (e: JSONException) {
   e.printStackTrace()
}

val tokens = JSONObject()
try {
   tokens.put("Budget", "CAD $300,000 to $600,000")
   tokens.put("ProjectName", "| New Development | Kings Landing Condos |")
} catch (e: JSONException) {
   e.printStackTrace()
}

NotifyVisitorsApi.getInstance(activityContext).show(tokens, customObj, "thirdFragment", object : OnInAppTriggerListener {
       override fun onDisplay(response: JSONObject) {
           //do your task here
       }
})
```

This allows the SDK to evaluate whether any In-App campaign should be displayed on that screen.

---

<br>

## Display Callback Responses

| Status | Message | Type | Notifications Shown | Notifications Not Shown |
|--------|---------|------|---------------------|-------------------------|
| **Success** | Found some active & inactive notifications. | `19.1` | `[133, 168]` or `[]` | `[146]` or `[]` |
| **Fail** | No internet found. | `19.0` | — | — |
| **Fail** | No banner/survey is active. If a campaign is active on the NVECTA Dashboard, verify that your application is running in the correct **DEBUG/LIVE** mode and that the campaign is configured for the same mode. | `19.3` | — | — |
| **Fail** | Something went wrong with error -> | `19.2`, `19.5`, `19.6` | — | — |
| **Fail** | No data found regarding any banner/survey. | `19.4` | — | — |

### Example UI Callback Response

```json
{
  "status": "success",
  "message": "Found some active & inactive notifications.",
  "type": 19.1,
  "notificationsShown": [133, 168],
  "notificationsNotShown": [146]
}
```
---
<br>

## In-app Notifications Response Callback

Register a callback to receive user interaction events from In-App Notifications, such as banner impressions, banner clicks, and survey responses

You can use this callback to:

- Handle banner clicks or CTA actions.
- Receive survey submission events.
- Perform custom actions based on user interactions.
- Debug or monitor in-app notification behaviour.

---

### Register the Callback

**Java**

```java
NotifyVisitorsApi.getInstance(this).getEventResponse(new OnEventTrackListener() {
    @Override
    public void onResponse(JSONObject response) {
       //do your task here

    }
});
```

**Kotlin**

```kotlin
NotifyVisitorsApi.getInstance(this).getEventResponse(object : OnEventTrackListener {
    override fun onResponse(response: JSONObject?) {
        //do your task here
    }
})
```

---

## Sample Response

```json
{
  "status": "success",
  "message": "Survey submitted successfully",
  "callbackType": "survey",
  "eventName": "Survey Submit",
  "attributes": {
    "notificationID": "2628",
    "rating": "9"
  },
  "type": 14
}
```

---

## Response Fields

| Field | Description |
|-------|-------------|
| `status` | Indicates whether the SDK processed the interaction successfully. |
| `message` | Additional information about the callback result. |
| `callbackType` | Type of callback, such as `banner`, `survey`, or another in-app interaction. |
| `eventName` | Name of the interaction performed by the user. |
| `attributes` | Additional data associated with the interaction, such as notification ID, survey response, or custom values. |
| `type` | Internal SDK callback identifier used for debugging and to distinguish similar callbacks originating from different SDK components. |

---

## Possible Callback Events

In the above output, the parameters have different values depending on the scenario that occurs when displaying banners or surveys. Below are the different values you can get in the callback:

| Status | Event Name | Message | Type | Callback Type |
|----------|------------|----------|----------|----------|
| Success | Banner Impression | InApp banner shown. | `15.12`, `15.13`, `15.14`, `15.17`, `15.18` | banner |
| Success | Banner Clicked | InApp Banner clicked. | `15.0` to `15.11`, `15.15`, `15.16` | banner |
| Success | Survey Attempt | Survey attempted successfully. | `14.0`, `14.2` | survey |
| Success | Survey Submit | Survey submitted successfully. | `14.1`, `14.3` | survey |

The values shown below are generated by the SDK and are intended primarily for debugging and troubleshooting. Applications should rely on `status`, `callbackType`, and `eventName` rather than specific `type` values.

> **Note**
>
> This callback reports **SDK interaction events** only. It is triggered when users interact with an In-App Notification (such as clicking a banner or submitting a survey) and can be used to update your application's UI, perform navigation, or collect diagnostic information.

---

<br>

# Best Practices

- Initialize the SDK before calling `show()`.
- Call `show()` once when a screen becomes visible.
- Track users for personalized campaigns.
- Track custom events for event-based targeting.
- Use meaningful fragment names when working with multiple fragments.
- Handle callbacks to monitor notification display and user interactions.
- Test campaigns using a test device before publishing.

<br> 

# Related Documentation

- 📊 Track Events
- 👤 Track Users
- 🌍 Global Attributes
- 🎯 In-App Nudges
- 🔔 Push Notifications
- 🔗 Deep Links