---
name: Provision a user and grant a classroom license
description: Create a CodeCombat user via the Partner API and grant them a Classroom license so they can join classes and play paid courses.
api: openapi/codecombat-openapi-original.yml
operations:
  - UsersService.create
  - UsersService.grantLicense
  - UsersService.get
---

# Provision a user and grant a classroom license

Use this to onboard a student or teacher into CodeCombat from a partner LMS.

## Auth
Every request uses HTTP Basic authentication with your partner API key pair against base URL `https://codecombat.com/api`. See `conventions/codecombat-conventions.yml`.

## Steps

1. **Create the user** — `UsersService.create` (`POST users`). Send `name` and `email` (both required). Set `role` to `"student"` or `"teacher"`; omit it for a Home user (which cannot join classrooms). Optionally set `preferredLanguage`, `heroConfig`, and `birthday`. The response is a `UserResponse` — capture `_id`.

2. **Grant a Classroom license** — `UsersService.grantLicense` (`PUT users/{handle}/license`) with the user's `_id` as `handle`. Body requires `ends` (a datetime string) for when the license expires. This sets the user's role to `"student"` and gives Classroom access.

3. **Verify** — `UsersService.get` (`GET users/{handle}`) to confirm `license.active` is true. Add `includePlayTime` if you need `stats.playTime`.

## Notes
- `handle` accepts the user `_id` or `slug`.
- To wind a license down early use `UsersService.shortenLicense`; for Home premium use `grantPremiumSubscription` / `shortenSubscription`.
- No idempotency-key contract exists — guard against duplicate creation by looking up the user first with `UsersService.lookup`.
