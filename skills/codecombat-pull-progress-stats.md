---
name: Pull classroom progress and playtime stats
description: Retrieve per-member stats, per-member level sessions, and aggregate playtime from the CodeCombat Partner API.
api: openapi/codecombat-openapi-original.yml
operations:
  - ClassroomsService.getMembersStats
  - ClassroomsService.getLevelsPlayed
  - StatsService.getPlaytimeStats
---

# Pull classroom progress and playtime stats

## Auth
HTTP Basic with your partner API key pair against `https://codecombat.com/api`.

## Steps

1. **Member stats for a classroom** — `ClassroomsService.getMembersStats` (`GET classrooms/{classroomHandle}/stats`). Optionally narrow output with `project` (comma-separated, e.g. `creator,playtime,state.complete`). Paginate with `memberLimit` (default 10, max 100) and `memberSkip`. Returns an array of `{ _id, stats: { gamesCompleted, playtime } }`.

2. **Levels played by one member** — `ClassroomsService.getLevelsPlayed` (`GET classrooms/{classroomHandle}/members/{memberHandle}/sessions`). Returns an array of `LevelSessionResponse` (state.complete, levelID like `wakka-maul`, playtime, dateFirstCompleted).

3. **Aggregate playtime across owned users** — `StatsService.getPlaytimeStats` (`GET playtime-stats`). Optional filters: `startDate`, `endDate` (user creation window) and `country`. Returns `{ playTime, gamesPlayed }`. For license consumption use `StatsService.getLicenseStats` (`GET license-stats`).

## Notes
- `handle` params accept `_id` or `slug`.
- Pagination is offset-based; page with `memberSkip` for classrooms over 100 members.
