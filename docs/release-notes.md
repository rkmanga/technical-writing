<section class="section" markdown="1">

<h2 class="section-title"><span class="icon-holder"><i class="fas fa-clipboard-list"> Release Notes</i></span></h2>

<div class="annotation-block" style="background: #11-0602; border: 0px solid #c3ddd5; border-radius: 4px; padding: 18px 22px; margin-bottom: 32px;">
  <h4 style="margin-top: 0; color: #02ffff; font-size: .75rem;">About This Sample</h4>
  <table style="width: 100%; font-size: 0.88rem; border-collapse: collapse;">
    <tr>
      <td style="width: 130px; font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Product</td>
      <td style="padding: 4px 0; color: #11-0602;"><strong>PulseAPI</strong> — a fictional REST API monitoring and alerting SaaS platform used by engineering teams to track uptime, latency, and error rates across their API endpoints.</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Audience</td>
      <td style="padding: 4px 0; color: #11-0602;">Existing PulseAPI customers — primarily backend developers and engineering team leads — who need to understand what changed, whether action is required, and how to take advantage of new features.</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Tools Used</td>
      <td style="padding: 4px 0; color: #11-0602;">Markdown, Git changelog (conventional commits), Jira (for linking issues to release items), VS Code.</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Challenge Solved</td>
      <td style="padding: 4px 0; color: #11-0602;">Release notes often serve two very different readers: developers who need precise technical detail, and team leads who want to know the business impact quickly. A second challenge was surfacing deprecation notices prominently enough that customers act on them before the removal date, without burying them in a long list of less urgent items.</td>
    </tr>
  </table>
</div>

---

!!! warning
    The `v1/monitors` endpoint is deprecated in this release and will be removed on **June 30, 2025**. Migrate to `v2/monitors` before that date. See <a href="#deprecations">Deprecations</a> for details.

## PulseAPI Release Notes — v2.3.0

**Release date:** March 10, 2025 &nbsp;|&nbsp; **API version:** `2025-03-10`

---

### Summary

Version 2.3 delivers multi-region monitoring, a redesigned alerting engine with support for composite alert conditions, and significant performance improvements to the status page embed. This release also begins the sunset of the v1 Monitors API.

---

### New Features

#### Multi-Region Monitor Support

You can now configure monitors to run checks from up to **5 geographic regions simultaneously** — US East, US West, EU West, Asia Pacific, and South America. Regional results appear separately in the dashboard and in webhook payloads, making it easier to identify region-specific latency or availability issues.

**How to enable:** In your monitor settings, open the **Regions** tab and select one or more regions. Monitors created via the API now accept a `regions` array:

```json
{
  "name": "Production API Health",
  "url": "https://api.example.com/health",
  "interval": 60,
  "regions": ["us-east", "eu-west", "ap-southeast"]
}
```

#### Composite Alert Conditions

The alerting engine now supports **AND/OR logic** across multiple conditions. Previously, alerts could only trigger on a single threshold (e.g., response time > 2000ms). You can now create rules such as:

!!! note
    If response time > 1500ms **AND** error rate > 5% **for 3 consecutive checks**

Configure composite conditions in **Alerts > New Alert Rule > Advanced Conditions**, or via the updated `/v2/alerts` endpoint.

#### Status Page Embed Performance

The embeddable status widget (`<script src="https://cdn.pulseapi.io/embed.js">`) now loads ~60% faster due to migrating from a polling architecture to server-sent events (SSE). No changes are required on your end — existing embeds update automatically.

#### Webhook Retry Dashboard

A new **Webhook Delivery Logs** page under Settings shows the delivery status of each webhook event, including failed attempts and retry history. You can now manually retry a failed delivery from this page without re-triggering the original event.

---

### Improvements

- **Dashboard load time** reduced by ~40% for accounts with more than 50 monitors, due to pagination and lazy loading of historical charts.
- **Incident timeline** now displays check-level granularity (individual check pass/fail results) rather than aggregate windows, giving more precise incident start and end times.
- **API rate limit headers** now included on all responses: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, and `X-RateLimit-Reset` (Unix timestamp).
- **Email digest format** updated to group alerts by monitor rather than by time, reducing noise for teams with high-frequency monitors.

---

### Bug Fixes

| Issue | Description |
|-------|-------------|
| `PLS-1842` | Fixed: SSL certificate expiry alerts were firing 30 days early for certificates using ECDSA keys |
| `PLS-1907` | Fixed: Webhook payloads for `monitor.recovered` events were missing the `duration_seconds` field |
| `PLS-1933` | Fixed: Status page embeds did not reflect manual incident updates within the expected 30-second window |
| `PLS-1954` | Fixed: Monitor creation via API returned `500` when the `timeout` field exceeded 30 seconds (now correctly returns `422` with a validation message) |

---

### Deprecations

<a name="deprecations"></a>

#### v1 Monitors API — Sunset June 30, 2025

The `/v1/monitors` endpoint family is **deprecated** as of this release. All v1 monitor endpoints will be **removed on June 30, 2025**.

**Affected endpoints:**

| Deprecated | Replacement |
|-----------|-------------|
| `GET /v1/monitors` | `GET /v2/monitors` |
| `POST /v1/monitors` | `POST /v2/monitors` |
| `PUT /v1/monitors/:id` | `PATCH /v2/monitors/:id` |
| `DELETE /v1/monitors/:id` | `DELETE /v2/monitors/:id` |

**What changed in v2:** The v2 Monitors API accepts the `regions` array (see New Features), uses `PATCH` instead of `PUT` for partial updates, and returns ISO 8601 timestamps instead of Unix integers in all response bodies. See the [v2 Migration Guide](https://docs.pulseapi.io/guides/migrate-v1-v2) for a full field-by-field comparison.

**No breaking changes to v2 Alerts or v2 Status Pages in this release.**

---

### Upgrade Notes

- **No action required** for the status page embed, dashboard improvements, or bug fixes — these changes are automatic.
- **Regional monitoring** is opt-in. Existing monitors continue running from the default US East region unless you update your configuration.
- **Composite alerts** are additive — existing single-condition alerts are not affected and continue to work without modification.
- **Migrate v1 monitor API calls** before June 30, 2025. Use the migration guide linked above to update your integration.

---

*Full changelog and API reference: [docs.pulseapi.io](https://docs.pulseapi.io)*

*PulseAPI is a fictional product created for portfolio demonstration purposes.*

</section>
