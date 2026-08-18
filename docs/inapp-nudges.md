# In-App Native Display Integration

This guide explains how to display **NVECTA Native Displays** (also known as **In-App Native Displays**, **Nudges**, or **Cards**) inside your Android application.

Official Documentation: <br>
https://www.nvecta.com/docs/in-app-native-display-integration

No prior knowledge is required. Follow each step in order.

<br>

# What is a Native Display?

A Native Display is an in-app message that appears as part of your application's user interface instead of a popup.

For example, you can display:

- Promotional banners
- Discount cards
- Product recommendations
- Welcome messages
- Offers
- Informational cards

Because the Native Display becomes part of your layout, it provides a seamless user experience.

<br>

# Prerequisites

Before integrating Native Displays, ensure that:

- The **NVECTA Android SDK v5.8.1 and later** on should be already integrated.
- The SDK is initialized (typically inside `Application.onCreate()`).
- A Native Display campaign has been created in the NVECTA Dashboard.
- You know the **Property ID** configured for the campaign.

<br>

> ⚠️ **Important**
>
> Always initialize the NVECTA SDK before trying to load a Native Display.

<br>

## Step 1: Add SDK Dependencies

Add the following dependencies to your app module's `build.gradle` file.

```gradle
dependencies {
    implementation "com.notifyvisitors.nudges:notifyvisitors-nudges:v0.0.9"
}
```

<br>

## Step 2: Import Required Classes

Import the following classes in your Activity or Fragment.

**Java**

```java
import com.notifyvisitors.nudges.NotifyVisitorsNativeDisplay;
import com.notifyvisitors.notifyvisitors.interfaces.OnNudgeUiCompletion;
import com.notifyvisitors.notifyvisitors.NotifyVisitorsApi;
```

**Kotlin**

```Kotlin
import com.notifyvisitors.nudges.NotifyVisitorsNativeDisplay
import com.notifyvisitors.notifyvisitors.interfaces.OnNudgeUiCompletion
import com.notifyvisitors.notifyvisitors.NotifyVisitorsApi
```

<br>

## Step 3: Create a Native Display View

Create a `NotifyVisitorsNativeDisplay` object using your **Activity Context**.

***Java**

```java
NotifyVisitorsNativeDisplay nativeDisplay = new NotifyVisitorsNativeDisplay(activityContext);
```

**Kotlin**

```kotlin
val nativeDisplay = NotifyVisitorsNativeDisplay(activityContext)
```

<br>

>
> ⚠️ **Important**
>
> Use an **Activity Context**.
>
> Do **not** use the Application Context.
>

<br>

## Step 4: Load Native Display Content

Call the `loadContent()` function.

**Java**

```java
nativeDisplay.loadContent(propertyId);
```
**Kotlin**

```kotlin
nativeDisplay.loadContent(propertyId)
```

---

### Parameter

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `propertyId` | String | ✅ Yes | Property ID configured in the NVECTA Dashboard |

---

### Example

**Java**

```java
nativeDisplay.loadContent("home");
```
**Kotlin**

```kotlin
nativeDisplay.loadContent("home")
```

---

Other examples:

**Java**

```java
nativeDisplay.loadContent("offer_banner");

nativeDisplay.loadContent("checkout_offer");

nativeDisplay.loadContent("profile_card");
```

**Kotlin**

```kotlin
nativeDisplay.loadContent("offer_banner")

nativeDisplay.loadContent("checkout_offer")

nativeDisplay.loadContent("profile_card")
```

>
> The Property ID must exactly match the one configured in the NVECTA Dashboard.
>

<br>

## Step 5: Receive Completion Callback

Once the Native Display has finished rendering, the SDK sends a callback.

Register the callback using:

**Java**

```java
NotifyVisitorsApi.getInstance(activityContext).nudgeUiFinalized(new OnNudgeUiCompletion() {
    @Override
    public void onFinish(JSONObject info) {
         //do your work here
    }
});
```
**Kotlin**

```kotlin
NotifyVisitorsApi.getInstance(this).nudgeUiFinalized(object : OnNudgeUiCompletion {
    override fun onFinish(info: JSONObject) {
         //do your work here
    }
})
```

<br>

### Callback Response

After the Native Display finishes processing, the SDK invokes the `onFinish()` callback and returns a JSON object containing the result.

Example:

```json
{
   "status":"fail",
   "message":"no data found",
   "size":{
      "height":"0",
      "width":"0"
   }
}
```

### Response Fields

| Field | Description |
|--------|-------------|
| `status` | Indicates the result of the request. Returns `success` when a Native Display is rendered, or `fail` when no matching campaign is available. |
| `message` | A descriptive message returned by the SDK. For example, `"Native display loaded successfully"` or `"no data found"`. |
| `size.height` | The final rendered height of the Native Display. Returns `0` if no content is available. |
| `size.width` | The final rendered width of the Native Display. Returns `0` if no content is available. |

<br>

> ℹ️ **Note:** A response with `"status": "fail"` and `"message": "no data found"` is **not an SDK error**. It simply means there is no eligible Native Display campaign available for the specified `propertyId` or the current user.
>

<br>

### Why is the Callback Important?

The Native Display size is determined only after the content has finished loading.

Using the callback allows your application to:

- Set the correct height
- Prevent empty space
- Avoid content being cropped
- Display dynamic campaigns correctly

<br>

## Step 6: Add the View to Your Layout

First, create a container in your layout.

Example:

```xml
<LinearLayout
    android:id="@+id/parent_view"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"/>
```

Then add the Native Display View.

**Java**

```java
val parentView = findViewById<LinearLayout>(R.id.parent_view)
NotifyVisitorsNativeDisplay nativeDisplay = new NotifyVisitorsNativeDisplay(this);
parentView.addView(nativeDisplay);
```

**Kotlin**

```kotlin
LinearLayout parentView = findViewById(R.id.parent_view);
val nativeDisplay = NotifyVisitorsNativeDisplay(this)
parentView.addView(nativeDisplay)
```

<br>

# Step 7: Complete Example

The following examples demonstrate the complete integration process.

The examples:

1. Create a Native Display view.
2. Add it to a parent layout.
3. Load the `"home"` Property ID.
4. Listen for the rendering callback.
5. Update the parent container height using the size returned by the SDK.

---

**Java**

```java
LinearLayout parentView = findViewById(R.id.parent_view);

// Create Native Display
NotifyVisitorsNativeDisplay nativeDisplay =new NotifyVisitorsNativeDisplay(this);

// Add to layout
parentView.addView(nativeDisplay);

// Load content
nativeDisplay.loadContent("home");

// Listen for rendering completion
NotifyVisitorsApi.getInstance(this).nudgeUiFinalized(new OnNudgeUiCompletion() {
    @Override
    public void onFinish(JSONObject info) {
        new Handler(Looper.getMainLooper()).post(() -> {
            try {
                JSONObject size = info.optJSONObject("size");
                if (size != null) {
                    String heightString = size.optString("height", "");
                    if (!heightString.isEmpty()) {
                        int height = Integer.parseInt(heightString);
                        ViewGroup.LayoutParams params = parentView.getLayoutParams();
                        params.height = height; 
                        parentView.setLayoutParams(params);
                    }
                }
            } catch (Exception e) {
                Log.e("NVECTA", "Failed to update height", e);
            }
        });
    }
});
```

**Kotlin**

```kotlin
val parentView = findViewById<LinearLayout>(R.id.parent_view)

// Create Native Display
val nativeDisplay = NotifyVisitorsNativeDisplay(this)

// Add to layout
parentView.addView(nativeDisplay)

// Load content
nativeDisplay.loadContent("home")

// Listen for rendering completion
NotifyVisitorsApi.getInstance(this).nudgeUiFinalized(object : OnNudgeUiCompletion {
    override fun onFinish(info: JSONObject) {
       Handler(Looper.getMainLooper()).post {
            try {
                val size = info.optJSONObject("size")
                val heightString = size?.optString("height").orEmpty()
                if (heightString.isNotEmpty()) {
                    val height = heightString.toInt()
                    parentView.layoutParams = parentView.layoutParams.apply {this.height = height}
                }
            } catch (e: Exception) {
                Log.e("NVECTA", "Failed to update height", e)
            }
        }
    }
})
```

<br>

## Recommended Layout

```xml
<LinearLayout
    android:id="@+id/parent_view"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"/>
```

<br>

# Best Practices

### ✅ Initialize the SDK first

Always initialize the NVECTA SDK before creating a Native Display.

---

### ✅ Use an Activity Context

Correct:

```java
new NotifyVisitorsNativeDisplay(this);
```

Avoid:

```java
getApplicationContext()
```

---

### ✅ Add the View before loading content

Correct order:

1. Create the view.
2. Add it to the parent layout.
3. Call `loadContent()`.

---

### ✅ Use the callback to update the layout

The final height is known only after rendering completes.

Always use the callback before resizing your container.

---

### ✅ Use `wrap_content`

Avoid fixed heights whenever possible.

```xml
android:layout_height="wrap_content"
```

---

### ✅ Use the exact Property ID

The Property ID in your code must match the one configured in the NVECTA Dashboard.

<br>

# Common Issues

## Nothing is displayed

Check the following:

- NVECTA SDK is initialized.
- Property ID is correct.
- Campaign is active.
- Campaign matches the current user.
- Device has an internet connection.

---

## Callback is not received

Verify that:

- `loadContent()` is being called.
- The SDK has completed initialization.
- The Activity is still active.

---

## Incorrect size

Ensure that:

- You wait for the callback before updating the layout.
- The parent container uses `wrap_content`.
- You apply the height returned by the SDK.

<br>

# Integration Flow

```
Initialize NVECTA SDK
            │
            ▼
Create NotifyVisitorsNativeDisplay
            │
            ▼
Add View to Layout
            │
            ▼
Call loadContent(propertyId)
            │
            ▼
SDK Downloads Campaign
            │
            ▼
Rendering Completed
            │
            ▼
onFinish() Callback
            │
            ▼
Read Height & Width
            │
            ▼
Update Parent Layout
            │
            ▼
Native Display Visible
```
<br>

# Related Documentation

Continue exploring other SDK features:

- 🎯 [In-App Notifications](/docs/inapp-integration.md)
- 📊 [Track Events](/docs/event-tracking-integration.md)
- 📊 [Track Users](/docs/user-tracking-integration.md)
- 🔗 [Deep Links](/docs/deep-link-handling.md)
- 🔔 [Push Notifications](/docs/push-integration.md)
