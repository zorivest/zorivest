# Cron Expressions

## What Are They?

A **cron expression** is a string that defines a schedule — when a task should run. Originally from Unix cron jobs, it uses 5 fields to specify minute, hour, day, month, and day-of-week.

---

## Format

```
┌───────────── minute (0–59)
│ ┌───────────── hour (0–23)
│ │ ┌───────────── day of month (1–31)
│ │ │ ┌───────────── month (1–12 or JAN–DEC)
│ │ │ │ ┌───────────── day of week (0–7, 0 and 7 = Sunday, or SUN–SAT)
│ │ │ │ │
* * * * *
```

---

## Special Characters

| Character | Meaning | Example |
|-----------|---------|---------|
| `*` | Any value | `* * * * *` = every minute |
| `,` | List | `1,15 * * * *` = minute 1 and 15 |
| `-` | Range | `1-5 * * * *` = minutes 1 through 5 |
| `/` | Step | `*/15 * * * *` = every 15 minutes |

---

## Common Examples

| Expression | Schedule |
|-----------|----------|
| `0 9 * * 1-5` | 9:00 AM, Monday through Friday |
| `0 */4 * * *` | Every 4 hours |
| `30 16 * * 1-5` | 4:30 PM on weekdays (market close) |
| `0 8 1 * *` | 8:00 AM on the 1st of each month |
| `*/30 9-16 * * 1-5` | Every 30 min during market hours, weekdays |
| `0 0 * * 0` | Midnight every Sunday |

---

## Timezone

Cron expressions run in the timezone configured on your policy. Zorivest defaults to your system timezone but allows override per policy (e.g., `America/New_York` for market hours).

---

## How Zorivest Uses It

Each **pipeline policy** has a cron expression that determines when it runs:
- The scheduler evaluates the cron against the policy's timezone.
- The next run time is displayed in the policy list.
- Policies must be approved before the scheduler will execute them.

---

## Quick Reference Tool

For testing cron expressions interactively, see [crontab.guru](https://crontab.guru/).
