# `simplefit_import 1.0` — content import format

## Overview

SimpleFit supports two kinds of JSON import:

- **Import JSON** (Settings → Local Backup → Import JSON) — a **destructive** full-backup restore. Every IndexedDB store is cleared before the file is written. Used to move between devices.
- **Import Content** (Settings → Local Backup → Import Content) — an **additive** content-only import. Adds exercises, routines, and health metrics alongside existing data. Never deletes, never overwrites. This document describes the file format.

The two paths use different file shapes and are not interchangeable. Content-import files carry a top-level `"simplefit_import": "1.0"` sentinel; the full-backup export has a `"version": 2` field instead.

## Top-level shape

```json
{
  "simplefit_import": "1.0",
  "exercises": [ ... ],
  "routines": [ ... ],
  "healthMetrics": [ ... ]
}
```

- `"simplefit_import": "1.0"` — **required**. The import is rejected without it.
- `exercises`, `routines`, `healthMetrics` — all **optional**; include any combination. Each, when present, must be an array (may be empty).

## `exercises[]`

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `name` | string | yes | Non-empty. Unique case-insensitively. |
| `type` | `"weight"` or `"timed"` | no | Defaults to `"weight"`. |
| `muscleGroup` | string | no | Freeform (e.g. `"Legs"`, `"Chest"`). |
| `notes` | string | no | Coaching cues, form notes, etc. |

## `routines[]`

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `name` | string | yes | Non-empty. Unique case-insensitively. |
| `notes` | string | no | |
| `exercises` | array | yes | Non-empty. See below. |

Each entry inside a routine's `exercises[]`:

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `exerciseName` | string | yes | Must match (case-insensitively) either an exercise in the same file's `exercises[]` array, or an existing exercise in the app. |
| `defaultSets` | number | no | Default 3. |
| `defaultReps` | number | no | Default 10 for weight exercises; forced to 0 for timed. |
| `defaultWeight` | number | no | Default 0. |
| `defaultDuration` | number | no | Seconds. Only used for timed exercises; default 60. |

## `healthMetrics[]`

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `name` | string | yes | Non-empty. Unique case-insensitively. |
| `kind` | `"numeric"`, `"dual"`, or `"duration"` | yes | `"numeric"` stores a single value; `"dual"` stores two values (e.g. systolic/diastolic); `"duration"` stores a duration in the metric's unit. |
| `unit` | string | no | E.g. `"bpm"`, `"kg"`, `"hr"`. |

## Dedup / merge semantics

- Name matching is **case-insensitive**. `"Push-Up"` and `"push-up"` are the same exercise.
- Duplicates against existing app data are **skipped, not errored**. The result summary tells the user how many were skipped.
- Duplicates *within* the import file (between two entries of the same kind) are treated as a validation error — resolve them in the file before importing.
- Existing records are never modified. If you want to change an existing routine, edit it in the app.

## Validation rules

Validation is all-or-nothing. If any rule fails, no records are written and the user sees a descriptive error.

- The `"simplefit_import": "1.0"` sentinel must be present.
- `exercises`, `routines`, `healthMetrics` must be arrays when present.
- Each exercise must have a non-empty `name`; `type` if specified must be one of the allowed values.
- Each routine must have a non-empty `name` and a non-empty `exercises` array.
- Each routine-exercise entry's `exerciseName` must resolve to an exercise in the file or already in the app.
- Each health metric must have a non-empty `name` and a valid `kind`.

## Out of scope

The content-import format does **not** carry:

- Logged sessions (`sessions` / `sessionExercises`)
- Health readings (`healthReadings`)
- Deletions or updates to existing records

If you need to migrate a full dataset, use **Download JSON** / **Import JSON** instead.

## Examples

### Minimal — one exercise

```json
{
  "simplefit_import": "1.0",
  "exercises": [
    { "name": "Goblet Squat", "muscleGroup": "Legs", "notes": "Keep chest tall" }
  ]
}
```

### Full — exercises + routine + metric, mixing weight and timed

```json
{
  "simplefit_import": "1.0",
  "exercises": [
    { "name": "Barbell Back Squat", "muscleGroup": "Legs" },
    { "name": "Bench Press", "muscleGroup": "Chest" },
    { "name": "Plank", "type": "timed", "muscleGroup": "Core" }
  ],
  "routines": [
    {
      "name": "Beginner Full-Body A",
      "notes": "3x/week alternating with B",
      "exercises": [
        { "exerciseName": "Barbell Back Squat", "defaultSets": 3, "defaultReps": 5, "defaultWeight": 95 },
        { "exerciseName": "Bench Press",         "defaultSets": 3, "defaultReps": 5, "defaultWeight": 65 },
        { "exerciseName": "Plank",               "defaultSets": 3, "defaultDuration": 45 }
      ]
    }
  ],
  "healthMetrics": [
    { "name": "Resting Heart Rate", "kind": "numeric", "unit": "bpm" }
  ]
}
```

## How to import

1. In SimpleFit, go to **Settings → Local Backup**.
2. Tap **Import Content** and pick the `.json` file.
3. You'll see a result summary like `Added 3 exercises, 1 routine, 1 metric.` — or a validation error if the file was malformed.

For help generating a file, see the AI prompt templates in [`docs/ai-prompts/`](./ai-prompts/).
