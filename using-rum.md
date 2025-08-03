

## ✅ **How to Use Datadog RUM

### 1️⃣ **Create a RUM Application in Datadog**

* Go to **Datadog Console** → *UX Monitoring* → *RUM Applications*.
* Click **“New Application”**.
* Choose **Web** (for web apps).
* It will give you an **Application ID** and a JavaScript snippet.

---

### 2️⃣ **Install RUM SDK**

#### If using plain HTML/JS:

Add this to your HTML `<head>` tag:

```html
<script src="https://www.datadoghq-browser-agent.com/datadog-rum.js"></script>
```

---

### 3️⃣ **Initialize RUM in Code**

```js
window.DD_RUM && window.DD_RUM.init({
  applicationId: 'YOUR_APP_ID',
  clientToken: 'YOUR_CLIENT_TOKEN',
  site: 'datadoghq.com', // or 'datadoghq.eu' based on your region
  service: 'your-app-name',
  env: 'production',
  version: '1.0.0',
  sampleRate: 100, // % of sessions to track
  trackInteractions: true, // track clicks, scrolls
});
```

> ✅ Put this code in your **index.html** or app entry point (like `main.ts` in Angular).

---

### 4️⃣ **(Optional) Start Session Replay**

```js
window.DD_RUM.startSessionReplayRecording();
```

---

### 5️⃣ **(Optional) Track Custom Actions**

```js
window.DD_RUM.addAction('button_clicked', {
  button_name: 'Login',
  page: 'Login Page',
});
```

---

### 6️⃣ **Verify in Datadog**

* Go to **UX Monitoring → RUM Applications**.
* Open your app dashboard.
* You’ll see metrics like:

  * Page views
  * User sessions
  * Errors
  * Clicks
  * Performance (FCP, LCP, etc.)

---

## 📌 Summary

| Step | Action                              |
| ---- | ----------------------------------- |
| 1    | Create app in Datadog               |
| 2    | Add SDK to project                  |
| 3    | Initialize with your app ID & token |
| 4    | (Optional) Enable session replay    |
| 5    | (Optional) Add custom user actions  |
| 6    | Check data in Datadog dashboard     |









