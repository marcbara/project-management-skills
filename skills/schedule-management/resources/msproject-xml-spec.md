# Microsoft Project XML Export Specification

Field-level rules for generating XML that imports cleanly into MS Project or ProjectLibre. Follow this spec exactly — most import failures come from violating one of the critical field rules below.

The output must be a valid XML file conforming to the Microsoft Project XML schema (namespace: `http://schemas.microsoft.com/project`). The file must open directly in MS Project or ProjectLibre without errors.

## Project-level structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Project xmlns="http://schemas.microsoft.com/project">
  <SaveVersion>14</SaveVersion>
  <Name>Project Name</Name>
  <StartDate>YYYY-MM-DDTHH:MM:SS</StartDate>
  <FinishDate>YYYY-MM-DDTHH:MM:SS</FinishDate>
  <ScheduleFromStart>1</ScheduleFromStart>
  <CalendarUID>1</CalendarUID>
  <DefaultStartTime>08:00:00</DefaultStartTime>
  <DefaultFinishTime>17:00:00</DefaultFinishTime>
  <MinutesPerDay>480</MinutesPerDay>
  <MinutesPerWeek>2400</MinutesPerWeek>
  <DaysPerMonth>20</DaysPerMonth>
  ...
</Project>
```

## Critical field rules — the most common causes of import failure

1. **`<Start>` and `<Finish>` inside tasks, NOT `<StartDate>` / `<FinishDate>`.** At the `<Project>` level, use `<StartDate>` and `<FinishDate>`. Inside `<Task>`, the fields are `<Start>` and `<Finish>`. Using `<StartDate>` inside a task causes MS Project to show duration as 0.

2. **`<Manual>0</Manual>` on every task.** Without this, MS Project interprets tasks as manually scheduled, ignoring durations and dependencies.

3. **`<DurationFormat>7</DurationFormat>` on every leaf task.** Format 7 = days. Without it, MS Project may misinterpret the PT duration format.

4. **`<RemainingDuration>` must equal `<Duration>`** for tasks that have not started.

5. **Duration encoding:** ISO 8601 format: `PT[hours]H0M0S`. Conversion: 1 working day = 8 hours = `PT8H0M0S`. 5 days = `PT40H0M0S`. 20 days = `PT160H0M0S`.

6. **Do NOT nest `<Assignments>` inside `<Tasks>`.** If resource assignments are included, `<Assignments>` is a separate section at the same level as `<Tasks>`, not inside individual tasks.

## Mandatory fields per task type

| Field | Leaf task | Summary task | Milestone |
|-------|-----------|-------------|-----------|
| `<UID>` | Unique sequential from 0 | Yes | Yes |
| `<ID>` | Sequential from 0 | Yes | Yes |
| `<Name>` | Activity name | Phase name | Milestone name |
| `<Type>1</Type>` | Yes | Yes | Yes |
| `<IsNull>0</IsNull>` | Yes | Yes | Yes |
| `<Manual>0</Manual>` | Yes | Yes | Yes |
| `<Summary>` | 0 | 1 | 0 |
| `<Milestone>` | 0 | 0 | 1 |
| `<Start>` / `<Finish>` | Yes (YYYY-MM-DDTHH:MM:SS) | Do NOT include (auto-calculated) | Same date/time for both |
| `<Duration>` | PT[hours]H0M0S | Do NOT include | PT0H0M0S |
| `<DurationFormat>7</DurationFormat>` | Yes | No | Yes |
| `<RemainingDuration>` | Equal to Duration | No | PT0H0M0S |
| `<OutlineLevel>` | Hierarchy depth | Hierarchy depth | Hierarchy depth |
| `<OutlineNumber>` | WBS number | WBS number | WBS number |
| `<WBS>` | WBS code | WBS code | WBS code |
| `<ConstraintType>0</ConstraintType>` | Yes (0 = ASAP) | No | Yes |
| `<CalendarUID>1</CalendarUID>` | Yes | Yes | Yes |

## Dependencies (inside each `<Task>`)

```xml
<PredecessorLink>
  <PredecessorUID>[UID of predecessor]</PredecessorUID>
  <Type>[0=FF, 1=FS, 2=SF, 3=SS]</Type>
  <LinkLag>[lag in days x 4800, or 0]</LinkLag>
  <LagFormat>7</LagFormat>
</PredecessorLink>
```

## Calendar

Include a complete `<Calendars>` section with a Standard calendar (UID=1). Define all 7 days (DayType 1-7) with DayWorking and WorkingTimes. Default: Monday-Friday working, Saturday-Sunday non-working. Adjust if the user specifies a different work week.

## Project summary task

**Task UID=0 is always the project summary task** (OutlineLevel=0, Summary=1). This is mandatory.

## Quality verification before delivering

- [ ] Every activity in the schedule has a corresponding task in the XML
- [ ] Every task (except the first) has at least one predecessor
- [ ] Every leaf task (except the final milestone) is a predecessor of at least one other task
- [ ] No circular dependencies
- [ ] Milestones have zero duration
- [ ] Summary tasks have `<Summary>1</Summary>` and no duration/start/finish
- [ ] All leaf tasks use `<Start>` and `<Finish>` (NOT `<StartDate>` / `<FinishDate>`)
- [ ] All tasks have `<Manual>0</Manual>`
- [ ] UIDs are unique and sequential
- [ ] The XML is well-formed (all tags closed)
- [ ] No `<Resources>` or `<Assignments>` unless the user explicitly requests them
- [ ] Start/Finish dates are consistent with durations and dependencies
