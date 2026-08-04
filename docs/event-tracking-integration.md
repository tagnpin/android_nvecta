# Event Tracking Guide

This document explains how to track custom events in your android application using the NVECTA Android SDK.

Official Documentation: <br>
https://www.nvecta.com/docs/tracking-android-events

---

# What is Event Tracking?

Event tracking helps you record user actions performed inside your application.

Examples:

- App Open
- Login
- Signup
- Add To Cart
- Purchase
- Subscription
- Button Click
- Screen Visit

These events help in:

- Analytics
- User segmentation
- Personalized campaigns
- Push automation
- Conversion tracking

NVECTA automatically tracks some system events after SDK integration, while custom events can be tracked manually based on your business requirements.

---

<br>

# Basic Event Tracking Flow

```text
User Action
    ↓
Android App
    ↓
NVECTA SDK
    ↓
NVECTA Dashboard Analytics
```

Example:

```text
User clicks "Purchase"
    ↓
Track "purchase_completed" event
    ↓
Event visible in NVECTA dashboard
```

---

<br>

## Event Types

### System Events

These events are tracked automatically after the SDK is initialized.

Examples:

- App Installed (install)
- App Launched (app_launch)
- App Update (update)
- Session Started (session_start)
- Screen View (screen_view)

No additional code is required.

---

### Custom Events

Custom events allow you to track actions that are specific to your application.

Examples:

- Product Viewed
- Add to Cart
- Order Placed
- Payment Successful
- Subscription Started
- Quiz Completed

---


<br>

# Track a Simple Event

Use the `event()` method to track user actions.

**Java**

```java
NotifyVisitorsApi.getInstance(this).event("Product Viewed", null, null, "1");
```

**Kotlin**

```kotlin
NotifyVisitorsApi.getInstance(this).event("Product Viewed", null, null, "1")
```

---

<br>

# Event Method Parameters

| Parameter | Description |
|---|---|
| `event_name` | Name of the event |
| `attributes` | Additional event data |
| `lifeTimeValue` | Score/value associated with event |
| `scope` | Defines tracking frequency |

---

<br>

# Track Event with Attributes

Attributes help store additional information related to the event.

Example:

- Product Name
- Price
- Quantity
- Category
- Payment Method

**Java**

```java
JSONObject attributes = new JSONObject();

try {
    attributes.put("product_name", "Running Shoes");
    attributes.put("category", "Footwear");
    attributes.put("price", 2499);
    attributes.put("quantity", 1);
} catch (JSONException e) {
    e.printStackTrace();
}

NotifyVisitorsApi.getInstance(this).event("Add To Cart", attributes, null, "1");
```

**Kotlin**

```kotlin
val attributes = JSONObject().apply {
    put("product_name", "Running Shoes")
    put("category", "Footwear")
    put("price", 2499)
    put("quantity", 1)
}

NotifyVisitorsApi.getInstance(this).event("Add To Cart", attributes, null, "1")
```

---
<br>

# Lifetime Value (LTV)
Use **LTV** to assign a score or monetary value to an event.

Example:

- Purchase Amount
- Reward Points
- Revenue Generated

**Java**

```java
JSONObject attributes = new JSONObject();

try {
    attributes.put("product_name", "Running Shoes");
    attributes.put("category", "Footwear");
    attributes.put("price", 2499);
    attributes.put("quantity", 1);
} catch (JSONException e) {
    e.printStackTrace();
}

NotifyVisitorsApi.getInstance(this).event("Order Placed", attributes, "2499", "1");
```

**Kotlin**

```kotlin
val attributes = JSONObject().apply {
    put("product_name", "Running Shoes")
    put("category", "Footwear")
    put("price", 2499)
    put("quantity", 1)
}

NotifyVisitorsApi.getInstance(this).event("Order Placed", attributes, "2499", "1")
```

---

<br>

# Understanding Scope Values

The `scope` parameter controls how frequently an event should be tracked.

| Scope Value | Description |
|---|---|
| `"1"` | Track every time |
| `"2"` | Track once per session |

Example:

```java
NotifyVisitorsApi.getInstance(this).event("Login", null, null, "2");
```

---

<br>

# Tracking Complex Attributes

You can also pass nested objects and lists.

**Java**

```java
JSONObject address = new JSONObject();

address.put("city", "London");
address.put("country", "United Kingdom");

JSONObject attributes = new JSONObject();

attributes.put("customer_name", "John");
attributes.put("address", address);

NotifyVisitorsApi.getInstance(this).event("Profile Updated", attributes, null, "1");
```

**Kotlin**

```kotlin
val address = JSONObject().apply {
    put("city", "London")
    put("country", "United Kingdom")
}

val attributes = JSONObject().apply {
    put("customer_name", "John")
    put("address", address)
}

NotifyVisitorsApi.getInstance(this).event("Profile Updated", attributes, null, "1")
```

---

<br>

# Event Response Callback

Receive a callback indicating whether the SDK successfully queued the event for batch processing or encountered an SDK-level error.

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
       //do your task
   }
})
 ```

Example response:

```json
{
   "status":"success",
   "eventName":"app_launch",
   "message":"Event accepted for processing",
   "type":0,
   "attributes":{
      
   },
   "callbackType":"event"
}
```

## Event Callback Response Types

The SDK returns different response messages and type codes based on event tracking status.

| STATUS | MESSAGE | TYPE | CALLBACK TYPE |
|---|---|---|---|
| Success | Event accepted for processing | `0` | Event |
| Fail | Please check for Event Name, it shouldn't be NULL or EMPTY. | `1` | Event |
| Fail | Context not found. | `2.0`, `2.1`, `2.2` | Event |
| Fail | Invalid SCOPE value found. | `3.0`, `3.1`, `3.2` | Event |
| Fail | `<event_name>` event can be tracked once in `<scope>` days. It will get tracked after `<remaining_days>` days. | `4.0`, `4.1` | Event |
| Fail | `<event_name>` event can be tracked once in `<scope>` days. It will get tracked from tomorrow or later. | `5` | Event |
| Fail | Analytics / Event Status is inactive in the NV Panel. | `6.0`, `6.1`, `6.2` | Event |
| Fail | Wrong credentials found. Recheck the NotifyVisitors BrandID and Encryption Key in your app's manifest file. | `7` | Event |
| Fail | Something went wrong while processing via the API. | `8` | Event |
| Fail | No response available corresponding to this event. | `9.0`, `9.1` | Event |
| Fail | LIFE-CYCLE Events INACTIVE or account not upgraded. | `10` | Event |
| Fail | Conversion failed. | `11.0`, `11.1` | Event |
| Fail | Authentication error. | `12.0`, `12.1` | Event |
| Fail | Something went wrong while processing the response. | `13` | Event |
| Fail | No internet found. | `16.0`, `16.1` | Event |
| Fail | Attribution tracking is disabled in the NV Panel. | `17.0` | Event |

>**Type:** Identifies the internal SDK component or processing stage where the callback originated. Primarily intended for debugging and troubleshooting SDK-level errors.

---

<br>

# Verify Event Tracking From Android Studio Logcat

Open:

```text
Android Studio → Logcat
```

Filter using:

```text
NotifyVisitors
```

Check for logs like:

```text
EVENT !!
EVENT NAME : <event_name> !! ATTRIBUTES : {"attr1":"val1"}
EventName = <event_name>, EventAttr(s) = {"attr1":"val1"}, LifeTimeVal = 7, Scope = 1
```

---

<br>

# Best Practices

- Use meaningful and descriptive event names, such as `order_placed`, `add_to_cart`, or `product_viewed`.
- Follow a consistent naming convention throughout your application (for example, use lowercase with underscores for all event and attribute names).
- Keep attribute names consistent across events (for example, always use `product_id` instead of mixing `productId`, `id`, or `item_id`).
- Maintain consistent data types for attributes (for example, always send `price` as a number and `quantity` as an integer).
- Include only relevant attributes that improve analytics, segmentation, and campaign targeting.
- Avoid sending sensitive or personally identifiable information (PII), such as passwords, payment details, or confidential user data.
- Use **Scope = 2** only for events that should be tracked once per session, such as `login` or `app_opened`.

---

<br>

# Recommended Event Naming Examples

| Action | Recommended Event |
|---|---|
| App Open | `app_launch` |
| Login | `user_login` |
| Signup | `user_signup` |
| Add To Cart | `add_to_cart` |
| Purchase | `purchase_completed` |
| Subscription | `subscription_started` |

---

<br>

# Common Use Cases

## 🛍️ E-commerce

Track when a user adds a product to their cart.

**Java**

```java
JSONObject attributes = new JSONObject();
attributes.put("product_name", "Sneakers");
attributes.put("price", 4999);
attributes.put("quantity", 1);

NotifyVisitorsApi.getInstance(this).event("add_to_cart", attributes, "4999", "1");
```
**Kotlin**

```kotlin
val attributes = JSONObject().apply {
    put("product_name", "Sneakers")
    put("price", 4999)
    put("quantity", 1)
}

NotifyVisitorsApi.getInstance(this).event("add_to_cart", attributes, "4999", "1")
```

---

## 👤 User Registration

Track when a new user signs up.

**Java**
```java
JSONObject attributes = new JSONObject();
attributes.put("method", "Google");

NotifyVisitorsApi.getInstance(this).event("user_signup", attributes, null, "2");
```

**Kotlin**
```kotlin
val attributes = JSONObject().apply {
    put("method", "Google")
}

NotifyVisitorsApi.getInstance(this).event("user_signup", attributes, null, "2")
```
---

<br>

# Summary

With NVECTA Android SDK event tracking, you can:

- Monitor user activity
- Analyze customer behavior
- Create personalized campaigns
- Trigger automations
- Improve user engagement

# Next Steps

Continue exploring the SDK:

- 👤 Track Users
- 🔔 Push Notifications
- 💬 In-App Notifications & Nudges
- 📦 User Properties
- 🌍 Global Attributes
- 🔗 Deep Links