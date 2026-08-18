# 👤 User Tracking Guide

User tracking helps you identify users and associate their profile information with events, campaigns, and analytics.

Once a user is identified, all future events and interactions are linked to that user profile.

**Official Documentation:** <br>
https://www.nvecta.com/docs/flutter-tracking-users

---

## What is User Tracking?

User tracking allows you to associate app activity, events, purchases, and engagement data with a specific user profile.

By identifying users, you can:

- Build unified customer profiles
- Personalize campaigns and notifications
- Track user journeys across sessions
- Segment users based on behavior
- Measure retention and engagement

In NVECTA, user profiles are created automatically and can later be linked to known user information such as email, mobile number, customer ID, or other identifiers.

---

<br>

# Basic User Tracking Flow

```text
User Registers / Logs In
          ↓
Android Application
          ↓
Identify User
          ↓
NVECTA User Profile Updated
          ↓
Events & Campaigns Linked To User
```

Example:

```text
User signs up
      ↓
Set Email & Mobile Number
      ↓
NVECTA creates/updates user profile
      ↓
Future events are mapped to the same user
```
---
<br>

# Create User Profile

Assign a unique identifier to a user after they sign in or register.

> **Note**
>
> Use a unique and stable identifier (such as your application's User ID or Customer ID). Avoid using temporary values that may change over time.

**Java**

```java
JSONObject params = new JSONObject();
try {
   params.put("userID", "78-ASD");
   params.put("name", "Andrew");
   params.put("email", "andrew@gmail.com");
   params.put("mobile", "9999999999");
} catch (JSONException e) {
   e.printStackTrace();
}

NotifyVisitorsApi.getInstance(activityContext).userIdentifier(params, new OnUserTrackListener() {
    @Override
    public void onResponse(JSONObject response) {
        //do your task here
    }
});
```

**Kotlin**

```kotlin
val attributes = JSONObject()
attributes.put("userID", "78-ASD")
attributes.put("name", "John")
attributes.put("email", "john@gmail.com")
attributes.put("mobile", "9999999999")

NotifyVisitorsApi.getInstance(activityContext).userIdentifier(attributes, object : OnUserTrackListener {
       override fun onResponse(response: JSONObject) {
           //do your task here
       }
})
```

---

<br>

# Common User Attributes

The following attributes are commonly used while identifying users.

| Attribute | Description |
|------------|-------------|
| email | User email address |
| mobile | Mobile number |
| name | Full name |
| userID | Unique customer identifier |
| gender | User gender |
| city | User city |
| country | User country |
| subscription_type | Current plan |
| customer_type | Premium, Free, etc. |

---

<br>

# Track Additional User Attributes

You can enrich user profiles with custom attributes.

Typical attributes include:

- Gender
- City
- Country
- Subscription Plan
- Membership Level

**Java**

```java
JSONObject profile = new JSONObject();

try {
    profile.put("name", "John Doe");
    profile.put("email", "john@example.com");
    profile.put("mobile", "+91XXXXXXXXXX");
    profile.put("city", "New Delhi");
    profile.put("plan", "Premium");
} catch (JSONException e) {
    e.printStackTrace();
}

NotifyVisitorsApi.getInstance(activityContext).userIdentifier(params, new OnUserTrackListener() {
    @Override
    public void onResponse(JSONObject response) {
        //do your task here
    }
});
```

**Kotlin**

```kotlin
val profile = JSONObject().apply {
    put("name", "John Doe")
    put("email", "john@example.com")
    put("mobile", "+91XXXXXXXXXX")
    put("city", "New Delhi")
    put("plan", "Premium")
}

NotifyVisitorsApi.getInstance(activityContext).userIdentifier(attributes, object : OnUserTrackListener {
       override fun onResponse(response: JSONObject) {
           //do your task here
       }
})
```

---


## User Tracking Callback Response

| STATUS | MESSAGE | TYPE |
|----------|----------|----------|
| Success | User profile updated successfully | `0` |
| Fail | Invalid user data found | `1` |
| Fail | Context not found | `2` |
| Fail | Authentication failed | `3` |
| Fail | No internet connection found | `4` |
| Fail | User tracking disabled from panel | `5` |
| Fail | Internal processing error | `6` |

<br>

# User Profile Best Practices

### Identify Users After Login

Always call user tracking after successful login or signup.

```text
Login Success
      ↓
Track User
      ↓
Track Events
```

---

### Use Consistent Identifiers

Use the same email, mobile number, or customer ID across sessions.

```text
Good:
john@example.com

Bad:
john@gmail.com
john.doe@gmail.com
```

---

### Update User Properties When Changed

Whenever user details change, update the profile again.

Examples:

- Email updated
- Mobile number changed
- Subscription upgraded
- User moved to another city

---

<br>

# Common Use Cases

# Example: User Registration

**Java**

```java
JSONObject profile = new JSONObject();
profile.put("name", "John Doe");
profile.put("email", "john@example.com");
profile.put("userID", "USER_1001");

NotifyVisitorsApi.getInstance(activityContext).userIdentifier(profile, new OnUserTrackListener() {
    @Override
    public void onResponse(JSONObject response) {
        Log.d(“App”, “User Response => ” + response.toString());
    }
});
```

**Kotlin**

```kotlin
NotifyVisitorsApi.getInstance(this).setUserId("USER_1001")

val profile = JSONObject().apply {
    put("name", "John Doe")
    put("email", "john@example.com")
    put("userID", "USER_1001")
}

NotifyVisitorsApi.getInstance(activityContext).userIdentifier(profile, object : OnUserTrackListener {
       override fun onResponse(response: JSONObject) {
           Log.d(“App”, “User Response -> ” + response.toString())
       }
})
```
After this, any event tracked by the SDK becomes associated with the identified user profile.

---

<br>

# Example: Subscription Upgrade

### Java

```java
JSONObject profile = new JSONObject();
profile.put("plan", "Gold");

NotifyVisitorsApi.getInstance(activityContext).userIdentifier(profile, new OnUserTrackListener() {
    @Override
    public void onResponse(JSONObject response) {
        Log.d(“App”, “User Response => ” + response.toString());
    }
});
```

### Kotlin

```kotlin
val profile = JSONObject().apply {
    put("plan", "Gold")
}

NotifyVisitorsApi.getInstance(activityContext).userIdentifier(profile, object : OnUserTrackListener {
       override fun onResponse(response: JSONObject) {
           Log.d(“App”, “User Response -> ” + response.toString())
       }
})
```

---

<br>

# Verify User Tracking From Android Studio Logcat

Open:

```text
Android Studio → Logcat
```

Filter using:

```text
NotifyVisitors
```

Example logs:

```text
SET USER IDENTIFIER !!
!! ATTRIBUTES : {"name":"Ram"}
```

---

<br>

# Event Tracking After User Identification

## Overview

To ensure events are properly mapped to user profiles on the NVECTA panel, you must wait for the user identification callback response before triggering any events. 

**Important:** If you call event tracking and user identification in parallel, the SDK may not be able to merge the data correctly on the panel because the user profile hasn't been fully synchronized yet.

---

## Correct Implementation Flow

```text
User Login Triggered
       ↓
Call setUserIdentifier()
       ↓
Wait for Callback Response
       ↓
Check Response Status
       ↓
If Success: Track Events
       ↓
Events Get Mapped to User Profile
```

---

## Wait for User Identification Before Tracking Events

If you're identifying a user and immediately tracking an event (such as **Login**, **Sign Up**, or **Purchase**), always wait for the user identification callback before sending the event.

### ❌ Incorrect

Tracking an event immediately after calling `setUserIdentifier()` may cause it to be associated with an anonymous or incorrect user profile.

**Java**

```java
JSONObject userAttributes = new JSONObject();
userAttributes.put("plan", "Gold");

NotifyVisitorsApi.getInstance(context).userIdentifier(userAttributes, new OnUserTrackListener() {
    @Override
    public void onResponse(JSONObject response) {
        Log.d("App", "User Response => " + response.toString());
    }
});

// Event sent immediately
NotifyVisitorsApi.getInstance(this).event("login_successful", null, "0", "1");
```

**Kotlin**

```kotlin
val userAttributes = JSONObject()
userAttributes.put("plan", "Gold")

NotifyVisitorsApi.getInstance(context).userIdentifier(userAttributes, object : OnUserTrackListener {
       override fun onResponse(response: JSONObject) {
           Log.d("App", "User Response -> " + response.toString())
       }
})

// Event sent immediately
NotifyVisitorsApi.getInstance(this).event("login_successful", null, "0", "1")
```

---

### ✅ Correct

Wait until the user identification operation completes successfully before tracking any user-specific events.

**Java**

```java
JSONObject userAttributes = new JSONObject();
try {
   userAttributes.put("plan", "Gold");
} catch (JSONException e) {
   e.printStackTrace();
}

NotifyVisitorsApi.getInstance(this).setUserIdentifier(userAttributes, response -> {
    if ("success".equals(response.optString("status"))) {
        NotifyVisitorsApi.getInstance(this).event("login_successful", null, "0", "1");
    }
});
```

**Kotlin**

```kotlin
val userAttributes = JSONObject()
userAttributes.put("plan", "Gold")

NotifyVisitorsApi.getInstance(this).setUserIdentifier(userAttributes) { response ->
    if (response.optString("status") == "success") {
        NotifyVisitorsApi.getInstance(this).event("login_successful", null, "0", "1")
    }
}
```

> **Why is this important?**
>
> User identification and event tracking are asynchronous operations. Waiting for the user identification callback ensures that subsequent events are associated with the correct user profile.

> **Tip**
>
> When performing multiple SDK operations sequentially, always chain them using callbacks (or coroutines where supported) instead of executing them in parallel.

---

## Key Takeaways

1. **Always wait for `setUserIdentifier()` callback** before tracking events
2. **Check the response status** to ensure user identification was successful
3. **Verify response is not null** before accessing its properties
4. **Handle errors gracefully** with try-catch blocks
5. **Track events only after user sync completes** to ensure proper mapping on NVECTA panel

---

<br>

# Recommended Flow

```text
App Launch
    ↓
User Login
    ↓
Track User Profile
    ↓
Wait for Response
    ↓
Track Custom Events
    ↓
Send Push Notifications
    ↓
Create User Segments
```

---

<br>

# Best Practices

- Track users immediately after a successful login or sign-up.
- Always use a unique and permanent User ID from your application.
- Keep user attributes up to date whenever profile information changes.
- Use consistent attribute names throughout your application (for example, always use `email` instead of mixing `email`, `email_id`, or `userEmail`).
- Use custom attributes to improve user segmentation and personalized campaigns.
- Avoid sending sensitive or confidential information, such as passwords, OTPs, payment details, or authentication tokens.
- Wait for the user identification callback to confirm the operation before tracking dependent events or performing subsequent user-related actions.
- Always check the callback response status before proceeding with the next operation.
- When performing multiple SDK operations sequentially, use asynchronous callbacks (or `async/await` where supported) to ensure the correct execution order.

---

<br>

# 🆔 NotifyVisitors UID

The **NotifyVisitors UID (NV UID)** is a unique identifier automatically generated by the NVECTA platform for every app user, whether they are **identified** or **anonymous**.

This identifier is used internally by NVECTA to recognize users across sessions and can also be used within your application whenever a unique NVECTA user identifier is required.

You can view the NV UID for each user in the **NVECTA Dashboard → Mobile Push → Subscribers**.

---

## Get the NV UID

Use the following API to retrieve the current user's NV UID.

**Java**

```java
String nvUid = NotifyVisitorsApi.getInstance(this).getNvUid();
```

**Kotlin**

```kotlin
val nvUid = NotifyVisitorsApi.getInstance(this).getNvUid()
```

---

## Example

**Java**

```java
String nvUid = NotifyVisitorsApi.getInstance(this).getNvUid();

Log.d("NVECTA", "NV UID: " + nvUid);
```

**Kotlin**

```kotlin
val nvUid = NotifyVisitorsApi.getInstance(this).getNvUid()

Log.d("NVECTA", "NV UID: $nvUid")
```

Example output:

```text
NV UID: cb506278-d427-458a-976b-00748de49bdc
```

---

## Important Notes

- The NV UID is generated and managed automatically by the NVECTA platform.
- It is available for both anonymous and identified users.
- The returned value may be `null` or an empty string if the SDK has not yet generated or synchronized the NV UID.
- Check that the returned value is valid before using it in your application.

<br>

# Summary

User tracking enables NVECTA to create a unified customer profile by linking user attributes, events, purchases, and engagement activities to a single user.

With proper user identification, you can:

- Build customer profiles
- Segment audiences
- Personalize campaigns
- Improve retention
- Analyze customer behavior

**Remember:** Always wait for the user identification callback response before tracking events to ensure proper data synchronization and event mapping on the NVECTA panel.

<br>

# Related Documentation

Continue exploring other SDK features:

- 📊 [Track Events](/docs/event-tracking-integration.md)
- 🎯 [In-App Notifications](/docs/inapp-integration.md)
- 🎯 [In-App Native Nudges](/docs/inapp-nudges.md)
- 🔗 [Deep Links](/docs/deep-link-handling.md)
- 🔔 [Push Notifications](/docs/push-integration.md)