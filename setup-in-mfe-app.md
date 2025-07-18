# MFE-based Angular apps

## ✅ Goal

Integrate Datadog Real User Monitoring (RUM) into a micro frontend architecture, ensuring that:

* RUM is initialized only once (usually in the shell or host app).
* All MFEs can contribute custom actions, errors, or user details to Datadog.
* You avoid duplicate RUM initializations.

---

## ✅ Setup Strategy

| Role             | RUM Setup?                     |
| ---------------- | ------------------------------ |
| Shell (Host) | ✅ Yes (Initialize RUM here)    |
| Remote MFEs  | ❌ No (Do NOT reinitialize RUM) |


## 🛠 Step-by-Step Guide

### ✅ 1. Install in Host App

```bash
npm install @datadog/browser-rum --save
```

### ✅ 2. Create `src/datadog.ts` in Host App

```ts
import { datadogRum } from '@datadog/browser-rum';

datadogRum.init({
  applicationId: 'YOUR_APPLICATION_ID',
  clientToken: 'YOUR_CLIENT_TOKEN',
  site: 'datadoghq.com',
  service: 'angular-mf-shell',
  env: 'prod',
  version: '1.0.0',
  sampleRate: 100,
  sessionReplaySampleRate: 20,
  trackInteractions: true,
});

datadogRum.startSessionReplayRecording();
```

---

### ✅ 3. Import in `main.ts` of Host App

```ts
import './datadog'; // RUM starts here
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';

platformBrowserDynamic()
  .bootstrapModule(AppModule)
  .catch(err => console.error(err));
```

---

### ✅ 4. Expose Datadog globally (Optional, recommended)

In `datadog.ts` or `main.ts`:

```ts
(window as any).datadogRum = datadogRum;
```

Now child MFEs can safely call:

```ts
(window as any).datadogRum.addAction(...)
```

---

### ✅ 5. In Remote MFE (Do NOT install or re-init RUM)

In your Angular MFE, simply use Datadog via the global object:

```ts
(window as any).datadogRum?.addAction('viewed_dashboard', {
  timestamp: Date.now(),
  feature: 'user-dashboard'
});
```

✅ You can also log custom errors:

```ts
(window as any).datadogRum?.addError(new Error('MFE fetch failed'));
```

---

### ✅ 6. Set User Info (Once in Shell)

After login is complete in shell:

```ts
datadogRum.setUser({
  id: 'user_123',
  name: 'John Doe',
  email: 'john@example.com'
});
```

---

## ✅ Best Practices

| Best Practice                   | Why                |
| ------------------------------- | ------------------ |
| Initialize RUM only in host     | Avoid duplicates   |
| Share via global variable       | Let MFEs interact  |
| Set user info centrally         | Ensure consistency |
| Avoid duplicate imports in MFEs | Prevent size bloat |

---

## ✅ Example Use Case

Let’s say you have:

* `shell` (host app)
* `dashboard-mfe`
* `reports-mfe`

You can do:

```ts
// From dashboard-mfe
(window as any).datadogRum?.addAction('opened_dashboard', { userType: 'admin' });

// From reports-mfe
(window as any).datadogRum?.addError(new Error('Chart load failed'));
```
