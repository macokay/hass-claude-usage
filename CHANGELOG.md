# Changelog

All notable changes to this integration are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased] - under development and testing

> **Note:** This version is not yet released. Under active testing — do not rely on this for production use.

## [7.0.0] - 2026-06-12

First release under `macokay/hass-claude-usage`. Forked from
[`trickv/hass-claude-usage`](https://github.com/trickv/hass-claude-usage),
hardened and aligned to the maintainer's HACS conventions.

### Security

- **OAuth state validation is now strict.** The CSRF check previously skipped
  validation when the pasted authorization code contained no `#state` suffix.
  A missing or mismatched state is now rejected. The callback always returns the
  code as `code#state`, so this does not affect normal authentication.
- **All workflows declare least-privilege permissions** (`contents: read`,
  scoped up only where a job needs to write).
- **Third-party action pinned to a commit SHA** — the markdown link checker is
  no longer tracked via a mutable tag.

### Fixed

- **Malformed API responses no longer crash the flow.** `_exchange_code`,
  `_fetch_account_info`, token refresh, and the data update path now catch
  `ValueError` (raised by `resp.json()` on invalid JSON) in addition to
  `aiohttp.ClientError`, surfacing a clean error instead of an unhandled
  exception.

### Changed

- **Adopted semantic versioning.** Version is now set automatically from the
  release tag by `release.yml`; the `manifest.json` version is no longer bumped
  manually.
- **Linting/formatting consolidated to ruff only** — black and isort removed
  from `pyproject.toml`, `.pre-commit-config.yaml`, and CI.
- **CI restructured to the canonical workflow set**: `validate.yml` (HACS),
  `hassfest.yml`, `lint.yml`, `release.yml`, `stale.yml`.
- **`manifest.json`** re-pointed to the new repository, codeowner set to
  `@macokay`, and `dependencies` + `integration_type` keys added.
- **`hacs.json`** now declares a minimum Home Assistant version.

### Added

- `release.yml` — auto-bumps the manifest version and uploads the zip on release.
- `stale.yml`, `dependabot.yml`, issue templates, `FUNDING.yml`, `.gitignore`.

### Removed

- **`deploy.sh`** — hardcoded a single developer's machine path and used `sudo`.
- **`TODO`** and the markdown-link-check job (tracked via issues instead).

### Notes

- The OAuth scope (`org:create_api_key user:profile user:inference`) is fixed by
  Anthropic's client and grants more than read-only usage access. Tokens are
  stored in Home Assistant's `.storage` in clear text (HA standard). Treat HA
  backups as secrets — anyone with file access to them has full Claude account
  access.
