
# ✅ Datadog Setup in Angular (Frontend RUM – Real User Monitoring)

## 🛠 Step 1: Create a Datadog Account

1. Go to [https://app.datadoghq.com/signup](https://app.datadoghq.com/signup)
2. Sign up and get your **Client Token** and **Application ID** from:

   Integrations → Browser → RUM


## 🛠 Step 2: Install Datadog Browser SDK

Inside your Angular project folder:

```bash
npm install @datadog/browser-rum --save
```

## 🛠 Step 3: Configure Datadog in Angular

Create a file like `datadog.ts` or configure inside `app.component.ts` or `main.ts`.

### ✅ Example: `src/datadog.ts`

```ts
import { datadogRum } from '@datadog/browser-rum';

datadogRum.init({
  applicationId: 'YOUR_APPLICATION_ID',
  clientToken: 'YOUR_CLIENT_TOKEN',
  site: 'datadoghq.com', // or 'datadoghq.eu' if EU region
  service: 'my-angular-app',
  env: 'prod', // or 'dev', 'staging'
  version: '1.0.0',
  sampleRate: 100,
  sessionReplaySampleRate: 20,
  trackInteractions: true
});

datadogRum.startSessionReplayRecording();
```

---

## 🛠 Step 4: Import It in `main.ts`

### ✅ `src/main.ts`

```ts
import './datadog'; // Make sure this is above other imports
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';

platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.error(err));
```

---

## ✅ What You Get Automatically

Once set up, Datadog RUM collects:

* Page load time
* Route changes
* Clicks, scrolls
* JS errors & stack traces
* Console logs (optionally)
* Session replays (like Hotjar)

---

## 🛠 Step 5 (Optional): Add Custom User Info

You can associate logs or sessions with a user:

```ts
datadogRum.setUser({
  id: '123',
  name: 'John Doe',
  email: 'john@example.com'
});
```

Use this in `ngOnInit()` of a component after login.

---

## ✅ Optional: Send Custom Actions

```ts
datadogRum.addAction('clicked_buy_button', {
  productId: 'A123',
  price: 299
});
```

---

## ✅ Summary

| Task                      | Status |
| ------------------------- | ------ |
| Install RUM SDK           | ✅      |
| Configure in `datadog.ts` | ✅      |
| Initialize in `main.ts`   | ✅      |
| Start session replay      | ✅      |
| Add custom user info      | ✅      |

---

## 📺 View in Datadog

Go to:
**Datadog UI → RUM → Explorer or Session Replay**

You can see:

* Performance stats
* Errors per user
* Real user sessions & clicks

---

Would you like help integrating **log collection**, **backend APM**, or creating a **Datadog dashboard** for this Angular project?

