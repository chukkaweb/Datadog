### ✅ What is RUM in Datadog?

RUM (Real User Monitoring) helps you track and analyze user interactions with your web app in real time. It captures:

* Page loads
* Clicks
* Errors
* API requests
* User sessions and performance

---

### ✅ Steps to Set Up Datadog RUM

#### 1. **Create a RUM Application in Datadog**

* Go to **Datadog → RUM → Applications**
* Click **“New Application”**
* Fill:

  * **Application Name** (e.g., `MyApp`)
  * **Type**: Browser
* Datadog will generate a `client token` and `application ID` → Save them!

---

#### 2. **Install the RUM Browser SDK**

You can include RUM in two ways:

##### Option A: **CDN (Good for small or no-build projects)**

Add this inside the `<head>` of your `index.html`:

```html
<script src="https://www.datadoghq-browser-agent.com/datadog-rum.js"></script>
<script>
  window.DD_RUM && window.DD_RUM.init({
    applicationId: 'YOUR_APP_ID',
    clientToken: 'YOUR_CLIENT_TOKEN',
    site: 'datadoghq.com', // or 'datadoghq.eu' for EU users
    service: 'your-app-name',
    env: 'production', // or 'dev', 'staging'
    version: '1.0.0',
    sampleRate: 100,
    trackInteractions: true
  });

  window.DD_RUM && window.DD_RUM.startSessionReplayRecording();
</script>
```

---

##### Option B: **NPM + Webpack/Angular/React projects**

Install SDK via npm:

```bash
npm install @datadog/browser-rum
```

Then in your main entry file (like `main.ts` or `app.module.ts` in Angular):

```ts
import { datadogRum } from '@datadog/browser-rum';

datadogRum.init({
  applicationId: 'YOUR_APP_ID',
  clientToken: 'YOUR_CLIENT_TOKEN',
  site: 'datadoghq.com', // Use datadoghq.eu if in EU
  service: 'your-angular-app',
  env: 'production',
  version: '1.0.0',
  sampleRate: 100,
  trackInteractions: true,
});

datadogRum.startSessionReplayRecording();
```

---

### ✅ Features to Use

* **trackInteractions: true** → Tracks user clicks and activity.
* **startSessionReplayRecording()** → Records sessions to replay issues.
* **addAction()** → Manually log custom actions/events.

  ```ts
  datadogRum.addAction("Clicked on 'Submit'", { formName: "Signup" });
  ```
* **addError()** → Manually report custom errors:

  ```ts
  datadogRum.addError(new Error('Manual error!'), { source: 'custom' });
  ```

---

### ✅ Real-Time Use Case

📌 Suppose you want to track users submitting a feedback form:

```ts
onFeedbackSubmit() {
  datadogRum.addAction('Feedback form submitted', {
    userId: this.user.id,
    feedbackType: this.selectedType,
  });
}
```

---

### ✅ Verify in Datadog

Go to **RUM → Explorer**:

* Filter by service, env, action name
* See session replays, user journeys, and performance bottlenecks

---

### ✅ Final Tips

| Feature              | Purpose                              |
| -------------------- | ------------------------------------ |
| `trackInteractions`  | Auto-captures clicks, scrolls, forms |
| `startSessionReplay` | Replay real user sessions            |
| `addAction()`        | Custom event tracking                |
| `addError()`         | Custom error logging                 |
| `service`, `env`     | Helps separate apps/environments     |



### ✅ **Default Attributes in Datadog RUM**

These are the standard attributes Datadog collects **automatically** for each event (e.g., views, actions, errors, resources):

---

### 1. **Global Context Attributes**

* `application_id`: Your RUM app’s unique ID.
* `session_id`: Unique user session ID.
* `view.id`: Unique ID for the current page/view.
* `view.url`: URL of the current view.
* `view.name`: Optional name if set by developer.
* `view.referrer`: Previous view URL.
* `view.time_spent`: Time spent on the view (ms).

---

### 2. **User Device & OS Info**

* `device.type`: desktop, mobile, tablet.
* `device.brand`: e.g., Apple, Samsung.
* `device.model`: e.g., iPhone 13, Galaxy S21.
* `os.name`: Windows, macOS, Android, iOS.
* `os.version`: Version of the OS.

---

### 3. **Browser Info**

* `browser.user_agent`: Full user agent string.
* `browser.name`: e.g., Chrome, Firefox.
* `browser.version`: Browser version.

---

### 4. **Geolocation Info (Approximate)**

* `geo.continent`
* `geo.country`
* `geo.city`
* `geo.region`
* `geo.postal_code`

---

### 5. **Network Info**

* `network.client.ip`: IP address of the client (masked).
* `network.connection.type`: wifi, cellular, ethernet.
* `network.downlink`: Estimated bandwidth.
* `network.effective_type`: e.g., 4g, 3g.

---

### 6. **Session Info**

* `session.type`: user / synthetic.
* `session.has_replay`: true/false (if session replay is enabled).
* `session.is_sampled`: Whether the session is collected.

---

### 7. **Timing Attributes**

For resource loading, user actions, and custom timings:

* `duration`
* `start_time`
* `end_time`

---

### 8. **Error Attributes**

(when an error is logged)

* `error.message`
* `error.source`: network, console, agent.
* `error.stack`
* `error.type`

---

### ✅ **Tip**: You can also add **custom attributes** with:

```js
DD_RUM.addAction('button_click', {
  custom_attr1: 'value',
  page_section: 'login'
});
```
