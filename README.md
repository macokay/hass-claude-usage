<h1 align="center">Claude Usage</h1>

<p align="center">
  Home Assistant integration that monitors your Claude (Anthropic) subscription usage.
</p>

<p align="center">
  <a href="https://github.com/hacs/integration">
    <img src="https://img.shields.io/badge/HACS-Custom-orange.svg" alt="HACS Custom" />
  </a>
  <a href="https://github.com/macokay/hass-claude-usage/releases">
    <img src="https://img.shields.io/github/v/release/macokay/hass-claude-usage" alt="GitHub release" />
  </a>
  <a href="https://github.com/macokay/hass-claude-usage/blob/main/LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License" />
  </a>
</p>

<p align="center">
  <img src="images/screenshot.jpg" alt="Claude Usage screenshot" width="100%" />
</p>

<p align="center">
  <a href="https://www.buymeacoffee.com/macokay">
    <img src="https://img.shields.io/badge/Buy%20Me%20A%20Coffee-%23FFDD00.svg?logo=buy-me-a-coffee&logoColor=black" alt="Buy Me A Coffee" />
  </a>
</p>

---

## Features

- Session usage — current 5-hour utilization (%)
- Weekly usage — 7-day utilization across all models (%)
- Weekly Sonnet usage — 7-day Sonnet-specific utilization (%)
- Usage pace — whether you're on track to stay within your weekly limit
- Extra usage — credits consumed and monthly limit
- All reset times as timestamp sensors
- Auto token refresh — no manual re-authentication needed

---

## Requirements

| Requirement | Details |
|---|---|
| Home Assistant | 2024.11.0 or newer |
| Claude subscription | Pro or Max plan |

---

## Installation

### Automatic — via HACS

1. Open **HACS** in Home Assistant.
2. Go to **Integrations** → three-dot menu (⋮) → **Custom repositories**.
3. Add `https://github.com/macokay/hass-claude-usage` as **Integration**.
4. Search for **Claude Usage** and click **Download**.
5. Restart Home Assistant.

### Manual

1. Download the latest release from [GitHub Releases](https://github.com/macokay/hass-claude-usage/releases).
2. Copy `custom_components/hass_claude_usage/` to your `config/custom_components/` directory.
3. Restart Home Assistant.

---

## Configuration

1. Go to **Settings → Devices & Services → Add Integration**.
2. Search for **Claude Usage**.
3. Open the authorization URL shown in the config flow, log in with your Anthropic account, and paste the authorization code.

| Option | Description | Default |
|---|---|---|
| Update interval | How often to poll the usage API (seconds) | 300 |

---

## Data

### Entities

| Entity | Unit | Description |
|---|---|---|
| `sensor.session_usage` | `%` | 5-hour session utilization |
| `sensor.session_reset_time` | timestamp | When the session resets |
| `sensor.week_usage` | `%` | 7-day utilization, all models |
| `sensor.week_usage_pace` | `%` | Usage relative to time elapsed in the week |
| `sensor.week_reset_time` | timestamp | When the weekly limit resets |
| `sensor.weekly_sonnet_usage` | `%` | 7-day Sonnet utilization |
| `sensor.weekly_sonnet_reset_time` | timestamp | When the Sonnet weekly limit resets |
| `sensor.extra_usage_enabled` | — | Whether extra usage is enabled |
| `sensor.extra_usage` | `%` | Extra usage utilization |
| `sensor.extra_usage_credits` | credits | Credits consumed this month |
| `sensor.extra_usage_limit` | credits | Monthly credit limit |
| `sensor.api_error` | errors | 1 if the last API call failed, 0 otherwise |

### Update interval

Data is fetched on the configured interval (default 300 s). Anthropic rate-limits the usage API — bursting dozens of requests in a minute results in a ~24 hour backoff. Keep the default.

---

## Dashboard

A pre-built dashboard YAML is included in `dashboards/claude_usage.yaml`.

1. Go to **Settings → Dashboards → Add Dashboard**.
2. Open the three-dot menu → **Edit Dashboard** → three-dot menu → **Raw configuration editor**.
3. Paste the contents of `dashboards/claude_usage.yaml` and save.

---

## Known Limitations

- The OAuth scope is fixed by Anthropic's client and grants more than read-only access. Tokens are stored in Home Assistant's `.storage` in clear text (HA standard) — treat HA backups as secrets.
- The usage API endpoint is undocumented and may change without notice.

---

## Credits

- [trickv/hass-claude-usage](https://github.com/trickv/hass-claude-usage) — original integration this fork is based on
- [Anthropic](https://anthropic.com) — API provider

---

## License

MIT — see [LICENSE](LICENSE).
