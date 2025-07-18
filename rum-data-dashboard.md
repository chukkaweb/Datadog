

## ✅ Using Datadog RUM Data in Dashboards & Queries

### 🔍 1. **Search RUM Events**

#### Go to:

**UX Monitoring > RUM Explorer**

Here you can:

* Filter by:

  * `event.type` (e.g. `view`, `action`, `resource`, `error`, `long_task`)
  * `service`, `env`, `country`, `user.id`, `status_code`, etc.

#### 🧪 Example:

```
event.type:view AND service:my-angular-app AND env:prod
```

### 📊 2. **Create Dashboards from RUM Events**

#### Go to:

**Dashboards > New Dashboard > New Widget > Timeseries / Table / Top list etc.**

Then use the query bar with `@rum-*` fields.

---

### 📈 3. **Sample Queries on RUM Events**

#### ✅ Track Page Views:

```sql
event.type:view
```

#### ✅ Top 5 Slow Pages (By Load Time):

```sql
event.type:view
| avg(view.loading_time)
| group by view.url
| sort desc
| limit 5
```

#### ✅ Error Rate by Browser:

```sql
event.type:error
| count()
| group by usr.browser.name
```

#### ✅ Unique Users by Country:

```sql
event.type:view
| count_distinct(usr.id)
| group by geo.country
```

---

### 📂 4. **Group of RUM Events**

You can group by any attribute like:

* `view.url`
* `usr.id`
* `geo.country`
* `usr.browser.name`
* `network.client.ip`

#### 🧪 Example:

```sql
event.type:view
| avg(view.loading_time)
| group by usr.browser.name, geo.country
```

---

### 🧩 5. **Using Event Side Panel**

1. In RUM Explorer, click on any event row.
2. You'll see:

   * Event metadata (browser, user ID, session, device, errors)
   * Traced backend calls (if APM is enabled)
   * Resources used (images, scripts)
   * Session Replay link

✅ Helps debug what user did & when.

---

### 🎯 6. **Useful RUM Attributes in Queries**

| Attribute           | Description                              |
| ------------------- | ---------------------------------------- |
| `usr.id`            | Unique user ID                           |
| `view.url`          | Page URL                                 |
| `view.loading_time` | Page load time                           |
| `session.type`      | `user` or `synthetics`                   |
| `geo.country`       | Country of user                          |
| `usr.browser.name`  | Browser name                             |
| `event.type`        | Type: `view`, `action`, `resource`, etc. |

---

### 🎁 Example Use Case Dashboard

**Widget 1:** Avg Load Time by Page

```sql
event.type:view
| avg(view.loading_time)
| group by view.url
```

**Widget 2:** Number of JS Errors

```sql
event.type:error
| count()
```

**Widget 3:** Top Browsers

```sql
event.type:view
| count()
| group by usr.browser.name
```
