---
name: Create a classroom and enroll a student in a course
description: Create a CodeCombat classroom, add a member, and enroll that member in a course via the Partner API.
api: openapi/codecombat-openapi-original.yml
operations:
  - ClassroomsService.create
  - ClassroomsService.upsertMember
  - ClassroomsService.enrollUserInCourse
---

# Create a classroom and enroll a student in a course

## Auth
HTTP Basic with your partner API key pair against `https://codecombat.com/api`.

## Steps

1. **Create the classroom** — `ClassroomsService.create` (`POST classrooms`). Required body: `name`, `ownerID` (a teacher user `_id`), and `aceConfig` (e.g. `{ "language": "python" }`). The response `ClassroomResponseWithCode` includes `_id` and the join `code`.

2. **Add the student** — `ClassroomsService.upsertMember` (`PUT classrooms/{handle}/members`) with the classroom `_id` as `handle`. Body requires the classroom `code` and the student `userId`. Optionally cap the returned member list with `retMemberLimit` (default 1000).

3. **Enroll in a course** — `ClassroomsService.enrollUserInCourse` (`PUT classrooms/{classroomHandle}/courses/{courseHandle}/enrolled`). Path params are the classroom `_id` and course `_id`; body requires `userId`. If the course is paid, the user must hold an active license (see the provision-and-license skill) and must already be a classroom member.

## Notes
- Remove a member with `ClassroomsService.removeMember`; remove a course enrollment with `ClassroomsService.removeEnrolledUser`.
- List a user's classrooms with `UsersService.getClassrooms`.
