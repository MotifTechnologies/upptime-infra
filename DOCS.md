# Motif Technologies Upptime Docs

## 📡 Adding Monitoring URLs

Edit the `sites` section in `.upptimerc.yml`.

### HTTP Check (websites)
```yaml
sites:
  - name: My Service
    url: https://example.com
```

### TCP Check (VPN, SSH, or services without HTTP response)
```yaml
sites:
  - name: My Service
    url: example.com
    port: 443
    check: "tcp-ping"
```


## 🔔 Alert Rules

### Notification Channel
Alerts are sent via Slack Webhook.
The following 3 values must be registered as GitHub Repository **Secrets**:

| Secret Name | Value |
|---|---|
| `NOTIFICATION_SLACK` | `true` |
| `NOTIFICATION_SLACK_WEBHOOK` | `true` |
| `NOTIFICATION_SLACK_WEBHOOK_URL` | Your Slack Webhook URL |

### Alert Conditions

| Status | Condition | Message |
|--------|-----------|---------|
| 🟥 Down | Non-200 HTTP response or TCP connection failure | `[service] is down` |
| 🟨 Degraded | Response time degradation detected | `[service] has degraded performance` |
| 🟩 Recovery | Service back to normal | `[service] is back up` |

### Check Interval
- Endpoints are checked every **5 minutes** via GitHub Actions
- If an incident is **not resolved**, a repeated Slack alert is sent every **5 minutes** via `notify-ongoing.yml`
- Minimum interval is 5 minutes due to GitHub Actions scheduler limitations

### Incident Lifecycle
1. Downtime detected → Slack alert sent
2. GitHub Issue automatically created and assigned to `jay-mjkim`
3. **Unresolved incidents trigger a repeated Slack alert every 5 minutes**
4. Recovery detected → Slack recovery alert sent
5. GitHub Issue automatically closed
