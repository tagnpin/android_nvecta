# Notification Center (App Inbox)

The Notification Center (App Inbox) allows you to display previously received push notifications inside your application.

This feature is useful when users miss notifications or want to revisit older messages. Notification expiry and categorization can be managed from the NVECTA dashboard.

Official Documentation: <br>
https://www.nvecta.com/docs/configure-notification-center

---


# Open Notification Center

Use the following method to display the built-in Notification Center UI.

**Java**

```java
NotifyVisitorsApi.getInstance(activityContext).showNotifications(dismiss, appInboxInfo, new OnBuildUiListener() {
   @Override
   public void onCenterClose() {
       //do your task here
   }
});
```

**Kotlin**

```kotlin
NotifyVisitorsApi.getInstance(activityContext).showNotifications(dismiss, appInboxInfo, object : OnBuildUiListener {
    override fun onCenterClose() {
        //do your task here
    }
})
```

## Parameters

| Parameter      | Type | Description                                                                             |
| -------------- | ---- | --------------------------------------------------------------------------------------- |
| `appInboxInfo` | JSONObject  | Optional configuration for a customized inbox UI. Pass `null` to use the default inbox. |
| `dismiss`      | int  | Controls whether the inbox screen remains in memory after a notification click.         |

### Dismiss Values

| Value | Behavior                                                             |
| ----- | -------------------------------------------------------------------- |
| `0`   | Notification Center remains in memory.                               |
| `1`   | Notification Center is removed from memory after notification click. |

---

## Basic Example

**Java**

```java
NotifyVisitorsApi.getInstance(activityContext).showNotifications(0, null, new OnBuildUiListener() {
   @Override
   public void onCenterClose() {
       //do your task here
   }
});
```
**Kotlin**

```kotlin
NotifyVisitorsApi.getInstance(activityContext).showNotifications(0, null, object : OnBuildUiListener {
    override fun onCenterClose() {
        //do your task here
    }
})
```

---

<br>

# Customizing Notification Center Tabs

You can create up to three custom tabs in the Notification Center.

> **Important**
>
> The label names must exactly match the labels configured in the NVECTA dashboard.

## Syntax to be followed for tab configurations

**Java**
```java
NVCenterStyleConfig config = new NVCenterStyleConfig();
config.setFirstTabDetail(firstTabLabel (string), firstTabName (string));
config.setSecondTabDetail(secondTabLabel (string), secondTabName (string));
config.setThirdTabDetail(thirdTabLabel (string), thirdTabName (string));
config.setSelectedTabColor(selectTabColor (hexCode in string));
config.setUnSelectedTabColor(unSelectTabColor (hexCode in string));
config.setSelectedTabIndicatorColor(selectTabIndicatorColor (hexCode in string));
```

**Kotlin**

```kotlin
val config = NVCenterStyleConfig()
config.setFirstTabDetail(firstTabLabel: String, firstTabName: String)
config.setSecondTabDetail(secondTabLabel: String, secondTabName: String)
config.setThirdTabDetail(thirdTabLabel: String, thirdTabName: String)
config.setSelectedTabColor(selectTabColor: String (hexCode))
config.setUnSelectedTabColor(unSelectTabColor: String (hexCode))
config.setSelectedTabIndicatorColor(selectTabIndicatorColor: String (hexCode))
```

## Example

**Java**

```java
NVCenterStyleConfig config = new NVCenterStyleConfig();
config.setFirstTabDetail("offer", "Offer");
config.setSecondTabDetail("promotion", "Promotions");
config.setThirdTabDetail("other", "Others");
config.setSelectedTabColor("#FF0000");
config.setUnSelectedTabColor("#000000");
config.setSelectedTabIndicatorColor("#000000");

NotifyVisitorsApi.getInstance(activityContext).showNotifications(0, config, new OnBuildUiListener() {
   @Override
   public void onCenterClose() {
       //do your task here
   }
});
```

**Kotlin**

```kotlin
val config = NVCenterStyleConfig()
config.setFirstTabDetail("offer", "Offer")
config.setSecondTabDetail("promotion", "Promotions")
config.setThirdTabDetail("other", "Others")
config.setSelectedTabColor("#FF0000")
config.setUnSelectedTabColor("#000000")
config.setSelectedTabIndicatorColor("#000000")

NotifyVisitorsApi.getInstance(activityContext).showNotifications(0, config, object : OnBuildUiListener {
    override fun onCenterClose() {
        //do your task here
    }
})
```

---

<br>

# Get Unread Notification Count

You can retrieve the unread notification count and display it on a badge, bell icon, or notification indicator.

**Java**

```java
NVCenterStyleConfig config = new NVCenterStyleConfig();
config.setFirstTabDetail(firstTabLabel (string), firstTabName (string));
config.setSecondTabDetail(secondTabLabel (string), secondTabName (string));
config.setThirdTabDetail(thirdTabLabel (string), thirdTabName (string));
config.setSelectedTabColor(selectTabColor (hexCode in string));
config.setUnSelectedTabColor(unSelectTabColor (hexCode in string));
config.setSelectedTabIndicatorColor(selectTabIndicatorColor (hexCode in string));

NotifyVisitorsApi.getInstance(activityContext).getNotificationCenterCount(new OnCenterCountListener() {
   @Override
   public void getCount(JSONObject counts) {
       //do your task here
   }
}, config (Object));
```

**Kotlin**

```kotlin
val config = NVCenterStyleConfig()
config.setFirstTabDetail(firstTabLabel: String, firstTabName: String)
config.setSecondTabDetail(secondTabLabel: String, secondTabName: String)
config.setThirdTabDetail(thirdTabLabel: String, thirdTabName: String)
config.setSelectedTabColor(selectTabColor: String);
config.setUnSelectedTabColor(unSelectTabColor: String);
config.setSelectedTabIndicatorColor(selectTabIndicatorColo: String);

NotifyVisitorsApi.getInstance(activityContext).getNotificationCenterCount(object : OnCenterCountListener {
    override fun getCount(response: JSONObject) {
        //do your task here
    }
}, config)
```

## Example

**Java**

```java
NVCenterStyleConfig config = new NVCenterStyleConfig();
config.setFirstTabDetail("all", "All");
config.setSecondTabDetail("promotion", "Promotions");
config.setThirdTabDetail("offer", "Offers");

NotifyVisitorsApi.getInstance(activityContext).getNotificationCenterCount(new OnCenterCountListener() {
   @Override
   public void getCount(JSONObject counts) {
       //do you task here
   }
}, config);
```

**Kotlin**

```kotlin
val config = NVCenterStyleConfig()
config.setFirstTabDetail("all", "All")
config.setSecondTabDetail("promotion", "Promotions")
config.setThirdTabDetail("offer", "Offers")

NotifyVisitorsApi.getInstance(activityContext).getNotificationCenterCount(object: OnCenterCountListener {
    override fun getCount(it: JSONObject) {
        //do your task here
    }
}, config)
```

## Callback Response

```json
{
   "totalCount":5,
   "tabOneCount":3,
   "tabTwoCount":0,
   "tabThreeCount":2
}
```

---

<br>

# Get Notification Center Data

If you want to build your own custom App Inbox UI, you can retrieve notification data directly from the SDK.

**Java**

```java
NotifyVisitorsApi.getInstance(activityContext).getNotificationCenterData(new OnCenterDataListener() {
   @Override
   public void getData(JSONObject response) {
       //do your task here
   }
});
```

**Kotlin**

```kotlin
NotifyVisitorsApi.getInstance(activityContext).getNotificationCenterData(object : OnCenterDataListener {
    override fun getData(response: JSONObject) {
        //do your task here
    }
})

```

The callback returns notification data in JSON format, allowing you to:

* Create your own inbox UI.
* Design custom notification cards.
* Implement custom filtering and sorting.
* Build a fully branded notification center experience.

## Callback Response

```json
{
  "message": "notification(s) found",
  "notifications": [
    {
      "title": "Rich Title",
      "message": "Rich Message 😂 Rich Message 😂 Rich Message 😂 Rich Message 😂 Rich Message 😂",
      "summary": "",
      "pushType": "horizontal crousel",
      "icon": "https://s3.amazonaws.com/notifypush/images/default_icon_app_5509.png",
      "target": "4",
      "url": "9999999999",
      "btnTitleOne": "Click Me",
      "btnUrlOne": "com.tnpnv_.pnbcodeapp.MainActivity",
      "btnTargetOne": "0",
      "btnTitleTwo": "",
      "btnUrlTwo": "",
      "btnTargetTwo": "0",
      "time": "1 year ago",
      "sendTime": "2023-10-01 00:00:00",
      "notificationId": "179154",
      "richImageUrl": "https://pushimages.notifyvisitors.com/images/push_rich_icon_75820.jpg",
      "parameters": {
        "default": {
          "rich_image_url": "<p><img class=\"fr-draggable\" src=\"blob:https://mail.notifyvisitors.com/32f9e3f5-9146-4fcc-a927-3c65ad126695\" width=\"188\" height=\"94\"></p> <p>Hello @{name}@, your email-id is @{email}@.</p>",
          "banner_position": "1",
          "show_banner": "true"
        }
      },
      "campaignLabel": [
        "offer",
        "promotion",
        "confirm"
      ],
      "crousel": [
        {
          "imageUrl": "https://clientcdn.notifyvisitors.com/Axis+MF/14may/500x250(1).png",
          "imageTarget": "1",
          "imageTargetVal": "https://docs.notifyvisitors.com/"
        },
        {
          "imageUrl": "https://cdn3.notifyvisitors.com/blog/wp-content/uploads/2024/04/5-1.png",
          "imageTarget": "1",
          "imageTargetVal": "https://www.google.co.in/"
        },
        {
          "imageUrl": "https://cdn3.notifyvisitors.com/blog/wp-content/uploads/2024/04/4-1.png",
          "imageTarget": "0",
          "imageTargetVal": "com.tnpnv_.notifycodeapp.MainActivity"
        }
      ]
    }
  ]
}
```

In case, when you have no broadcasted push in the panel.

```json
{
   "message": "no notification(s)",
   "notifications": []
}
```

---

<br>

# Notification Categories

Notifications can be organized into multiple tabs using labels configured in the NVECTA dashboard.

Example categories:

```text
Offers
Promotions
Announcements
Updates
Transactions
```

Users can switch between tabs to view notifications belonging to specific categories.

---

<br>

# Common Use Cases

### Show Notification History

Allow users to revisit notifications they may have dismissed.

### Inbox Screen

Create a dedicated "Inbox" section within your app.

### Notification Badge

Display unread notification count on:

* Bell icons
* Navigation tabs
* Profile sections
* Dashboard widgets

### Custom Inbox UI

Use `getNotificationCenterData()` to build a completely custom notification center matching your application's design.

---

<br>

# Best Practices

* Create meaningful notification categories.
* Use unread counts to improve engagement.
* Keep important notifications available in the inbox even after dismissal.
* Use a custom inbox UI when you need complete control over the user experience.
* Configure notification expiry from the NVECTA dashboard to prevent outdated messages from appearing.

## Recommended Flow

```text
Push Notification Received
            │
            ▼
      Stored in Inbox
            │
            ▼
 User Opens App Inbox
            │
            ├── View Notifications
            ├── Filter by Category
            ├── Check Unread Count
            └── Open Notification Details
```

The Notification Center provides a persistent in-app repository of push notifications, ensuring users can access important messages even after they have been dismissed from the device notification tray.