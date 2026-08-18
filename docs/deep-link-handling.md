# Deep Linking

Deep linking allows users to open a specific screen in your app directly from an NVECTA notification, push notification, in-app message, email, SMS, or any other supported channel.

For example:

```text
Notification → Tap → My Orders → Order #12345
```

When a user taps a deep link, NVECTA forwards the configured URL to your application. Your app is responsible for recognizing the URL and navigating the user to the appropriate screen.

---

# Supported Deep Links

NVECTA supports forwarding any valid deep link configured in your campaign.

Common types include:

- **Android App Links (HTTPS)** — Recommended
- **Custom URI Schemes**

### Android App Links

Example:

```text
https://yourdomain.com/orders/12345
```

Android App Links provide the best user experience because they can:

- Open your app directly when installed.
- Open the website if the app is not installed.
- Work across notifications, emails, SMS, QR codes, and web pages.

### Custom URI Schemes

Example:

```text
myapp://orders/12345
```

Custom URI schemes work only if your application has registered the scheme and are generally recommended for app-specific use cases.

> **Recommendation:** Use Android App Links (`https://`) whenever possible.

---

# Prerequisites

Before using deep links with the NVECTA Plugin, ensure your application already supports deep linking.

Your application should have:

- NVECTA Plugin integrated.
- Deep link handling implemented in the application.
- Android App Links or Custom URI Schemes configured.

For Android App Links, this generally includes:

- Intent Filters
- `android:autoVerify="true"`
- `assetlinks.json` hosted on your domain

For complete implementation details, refer to the official Android documentation:

https://developer.android.com/training/app-links

> **Note**
>
> Configuring deep links is an application-level implementation and is outside the scope of the NVECTA Plugin.
>
> The NVECTA Plugin **does not create or configure deep links**. It simply forwards the configured URL to your application, allowing your existing deep-link routing to handle navigation.

---

# Using Deep Links with NVECTA

Once your application supports deep linking, simply provide the desired deep link while creating your campaign in the NVECTA Dashboard.

Example:

```text
https://yourdomain.com/orders/12345
```

When a user taps the notification:

```text
NVECTA Notification
        ↓
User taps notification
        ↓
NVECTA forwards the URL
        ↓
Android resolves the deep link
        ↓
Your application receives the URL
        ↓
Your app navigates to the appropriate screen
```

No additional implementation is required within the NVECTA Plugin for handling navigation.

---

# Application Responsibility

The application should:

- Receive the incoming deep link.
- Parse the URL.
- Extract any required parameters.
- Navigate to the correct screen.
- Gracefully handle invalid or unsupported URLs.

Example:

```text
Incoming URL

https://yourdomain.com/orders/12345
```

Your application may extract:

```text
Screen : Order Details
Order ID : 12345
```

and navigate the user accordingly.

---

# Testing

Before using deep links in an NVECTA campaign, verify that they already work within your application.

You can test Android App Links using ADB:

```bash
adb shell am start \
  -a android.intent.action.VIEW \
  -d "https://yourdomain.com/orders/12345"
```

If the correct screen opens, the same deep link can be used in your NVECTA campaign.

---

# Troubleshooting

### Notification opens the app but not the expected screen

This usually indicates that the application's deep-link routing logic is not correctly handling the incoming URL.

Verify that your application correctly parses and routes the configured deep link.

---

### Link opens in the browser

Check your Android App Link configuration.

Common causes include:

- Missing or incorrect Intent Filters
- `android:autoVerify="true"` not configured
- Missing or incorrect `assetlinks.json`
- Incorrect package name or signing certificate

---

### Notification opens but nothing happens

Verify that:

- The deep link configured in the NVECTA Dashboard is correct.
- Your application supports the configured URL.
- The application is correctly handling incoming deep-link intents.

---

# Recommended Approach

For the best user experience, use **Android App Links (HTTPS URLs)**.

Example:

```text
https://yourdomain.com/products/123
https://yourdomain.com/orders/12345
https://yourdomain.com/profile/789
```

Overall flow:

```text
NVECTA Campaign
        ↓
Notification contains Deep Link
        ↓
User taps notification
        ↓
NVECTA forwards the URL
        ↓
Android resolves the link
        ↓
Application receives the URL
        ↓
Application navigates to the destination screen
```

---

# Need Help?

If your application already supports deep linking but you encounter issues with URL redirection through the NVECTA Plugin, please contact the NVECTA support team.

> **Important**
>
> The NVECTA Plugin supports **deep-link redirection only**. The creation, configuration, verification, and routing of deep links are entirely managed by your application.