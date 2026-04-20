# Example session — designing a routine with Claude

This is a real interview session using [`unified-routine-design.md`](./unified-routine-design.md) to design a 3×/week full-body routine with Claude. The prompt guides Claude to interview the user about experience, goals, schedule, equipment, and limitations before producing a `simplefit_import 1.0` JSON file.

Use it to get a sense of:
- how granular the interview gets (one question at a time, follow-ups welcome)
- how user constraints (injuries, equipment, progression questions) shape the plan
- what the final JSON output looks like in practice

---

**Human:**

> use this prompt to design a routine for me

*(attached the contents of `unified-routine-design.md`)*

**Claude:**

> Let's build your routine! I'll ask one question at a time.
>
> First up: What's your experience level?
>
> - Complete beginner (little to no training history)
> - A few months of consistent training
> - A year or more of training
> - Advanced (3+ years, structured programming)

**Human:**

> I've had a few months consistent training.

**Claude:**

> Great, you've got a solid foundation to build on!
>
> What's your primary goal?

**Human:**

> Q: What's your primary goal?
> A: I'm trying to build muscle mass, but the overall goal is my longterm health. I've been told that I need to lose some weight, and also help bring down my blood pressure.

**Claude:**

> That's a great combination of goals — building muscle actually supports all three: it boosts metabolism for fat loss, and regular resistance training is well-documented for improving blood pressure over time. We'll design around hypertrophy with that bigger picture in mind.
>
> How many days per week can you train, and roughly how long is each session?

**Human:**

> Q: How many days per week can you train?
> A: 3 days
>
> Q: How long is a typical session?
> A: 45–60 minutes

**Claude:**

> Perfect — 3 days is ideal for a full-body routine, which is actually great for muscle building and cardiovascular health at your stage.
>
> What equipment do you have access to?

**Human:**

> Q: What equipment do you have available? (Select all that apply)
> A: Dumbbells, Barbell + plates, Machines, Cables, Kettlebells, Resistance bands, Bodyweight only

**Claude:**

> Great, you've got a full arsenal to work with — that gives us lots of good options!
>
> Do you have any injuries, limitations, or movements you need to avoid?

**Human:**

> Since I started exercising I've been getting cramps when I do crunches, though it's possible that's just because my core needs strengthening. Aside from that, if I exercise with bad form my shoulders can experience pain, so it's important to escalate resistance carefully.

**Claude:**

> Noted — we'll skip crunches and use core work that's gentler and more effective anyway (like dead bugs and planks). And we'll include form cues on any shoulder-loading movements and keep starting weights conservative.
>
> Last question — any exercise preferences or things you'd like to avoid?

**Human:**

> I think with form tips, I'd like guidance on how much to step up weights if we're starting too low. Like how much to add if I find an exercise easy, and how much I should be aiming to struggle at the end of the sets. In terms of anything to avoid, my gym does have machines but it's not like Planet Fitness where there's an infinite variety, so it would be good to use exercises that are simple or staples, or can be adapted to free weight if a machine isn't available.

**Claude:**

> Perfect guidance — I'll add progressive overload cues to every exercise note, and keep everything either barbell/dumbbell/cable-based so you're never stuck if a specific machine isn't free.
>
> Here's your routine — two alternating full-body sessions (A/B/A one week, B/A/B the next):

```json
{
  "simplefit_import": "1.0",
  "exercises": [
    {
      "name": "Barbell Back Squat",
      "type": "weight",
      "muscleGroup": "Legs",
      "notes": "Feet shoulder-width, bar on upper traps. Break at hips and knees together, keep chest tall, knees track over toes. Aim for parallel or below. Add 5 lb when your last 2 reps of the final set feel like you had 2+ more in you. Target: those last 2 reps should be genuinely hard but not form-breaking."
    },
    {
      "name": "Dumbbell Bench Press",
      "type": "weight",
      "muscleGroup": "Chest",
      "notes": "Elbows at ~45° from your torso — not flared wide — this protects the shoulder joint. Press up and slightly inward. Add 2.5–5 lb per dumbbell when all sets feel easy with 2+ reps in reserve. Shoulder comfort is non-negotiable: if anything pinches, reduce the weight."
    },
    {
      "name": "Dumbbell Bent-Over Row",
      "type": "weight",
      "muscleGroup": "Back",
      "notes": "Hinge forward ~45°, brace your core, pull the dumbbell toward your hip (not your armpit). Drive with the elbow, not the hand. Add 5 lb when all sets feel easy. Last 2 reps of the final set should feel like work."
    },
    {
      "name": "Dumbbell Romanian Deadlift",
      "type": "weight",
      "muscleGroup": "Hamstrings",
      "notes": "Hinge at hips with a soft knee bend until you feel a hamstring stretch, then drive hips forward to stand. Keep the dumbbells close to your legs throughout. Add 5 lb when all reps feel easy with 2+ in reserve. Never round the lower back — form always beats weight."
    },
    {
      "name": "Dead Bug",
      "type": "weight",
      "muscleGroup": "Core",
      "notes": "Lie on your back, arms pointing at the ceiling, knees at 90°. Slowly lower one arm and the opposite leg toward the floor while pressing your lower back into the ground. Return and switch sides — that's one rep. Move deliberately, 3–4 seconds each way. No crunching motion. Progress by adding reps (aim for 12 then 16 before adding any load)."
    },
    {
      "name": "Goblet Squat",
      "type": "weight",
      "muscleGroup": "Legs",
      "notes": "Hold a dumbbell or kettlebell at chest height. The weight counterbalances you and makes it easier to squat upright and deep — great for reinforcing good mechanics. Add 5 lb when all sets feel easy with 2+ reps in reserve."
    },
    {
      "name": "Dumbbell Overhead Press",
      "type": "weight",
      "muscleGroup": "Shoulders",
      "notes": "Start lighter than you think — shoulder health comes first. Press from shoulder height with elbows slightly in front of the body (not flared wide). Only add weight when you complete all reps with zero shoulder discomfort and 2+ reps in reserve. Add just 2.5 lb at a time on this one."
    },
    {
      "name": "Lat Pulldown",
      "type": "weight",
      "muscleGroup": "Back",
      "notes": "Slightly wider than shoulder-width grip. Pull the bar to your upper chest by driving your elbows down and back. Avoid swinging or using momentum. No machine available? Substitute dumbbell bent-over rows or a resistance band pull-down from a door anchor. Add 5–10 lb when all sets feel easy."
    },
    {
      "name": "Dumbbell Reverse Lunge",
      "type": "weight",
      "muscleGroup": "Legs",
      "notes": "Step back rather than forward — reverse lunges are easier on the knees. Keep your front knee tracking over your toes and your torso upright. Add 5 lb per dumbbell when all sets feel easy with 2+ reps to spare."
    },
    {
      "name": "Plank",
      "type": "timed",
      "muscleGroup": "Core",
      "notes": "Forearms and toes, body in a straight line from head to heels. Squeeze your glutes and brace your abs as if bracing for a punch. When you can hold the full duration comfortably, add 10–15 seconds next session."
    }
  ],
  "routines": [
    {
      "name": "Full Body A",
      "notes": "Alternate with Full Body B each session: week 1 is A/B/A, week 2 is B/A/B. Rest 60–90 seconds between sets. Universal progression rule: if the last 2 reps of your final set feel easy (2+ reps clearly left in the tank), add weight next session. You should be working hard on those last reps — controlled struggle is the goal, not grinding with broken form.",
      "exercises": [
        { "exerciseName": "Barbell Back Squat",          "defaultSets": 3, "defaultReps": 10, "defaultWeight": 0 },
        { "exerciseName": "Dumbbell Bench Press",        "defaultSets": 3, "defaultReps": 10, "defaultWeight": 0 },
        { "exerciseName": "Dumbbell Bent-Over Row",      "defaultSets": 3, "defaultReps": 10, "defaultWeight": 0 },
        { "exerciseName": "Dumbbell Romanian Deadlift",  "defaultSets": 3, "defaultReps": 10, "defaultWeight": 0 },
        { "exerciseName": "Dead Bug",                    "defaultSets": 3, "defaultReps": 8,  "defaultWeight": 0 }
      ]
    },
    {
      "name": "Full Body B",
      "notes": "Alternate with Full Body A each session. Same rest and progression rules apply. Pay extra attention to shoulder form on the overhead press — it's the one exercise where conservative weight increases matter most.",
      "exercises": [
        { "exerciseName": "Goblet Squat",            "defaultSets": 3, "defaultReps": 10, "defaultWeight": 0 },
        { "exerciseName": "Dumbbell Overhead Press", "defaultSets": 3, "defaultReps": 10, "defaultWeight": 0 },
        { "exerciseName": "Lat Pulldown",            "defaultSets": 3, "defaultReps": 10, "defaultWeight": 0 },
        { "exerciseName": "Dumbbell Reverse Lunge",  "defaultSets": 3, "defaultReps": 10, "defaultWeight": 0 },
        { "exerciseName": "Plank",                   "defaultSets": 3, "defaultReps": 1,  "defaultWeight": 0, "defaultDuration": 30 }
      ]
    }
  ]
}
```

> Save this as a `.json` file, then in SimpleFit open **Settings → App · Import Content** to load it.

> A few extra notes worth knowing:
>
> **On progression:** all weights start at 0 so you fill them in on day one — pick a weight where you finish the set feeling like you could have done 2–3 more reps. That's your baseline. From there, the notes in each exercise tell you exactly when and how much to add.
>
> **On blood pressure:** the 60–90 second rest periods and the compound movements (squat, deadlift, row) are particularly good for cardiovascular adaptation over time. If your doctor has cleared you for exercise, this style of training is well-supported for improving BP with consistency.
>
> **On the core cramps:** Dead Bug and Plank avoid the spinal flexion that tends to trigger cramping with crunches. As your core strengthens, you can gradually increase Dead Bug reps and Plank duration.

---

## Notes for future reference

- The interview ran six questions deep before Claude started generating. That's normal — the prompt instructs it not to produce JSON until it has enough context.
- The user volunteered constraints the prompt didn't ask about (cramps on crunches, shoulder pain on bad form, guidance on progression). Claude folded those into the exercise `notes` fields. If you have similar constraints, mention them up front.
- Claude proposed two routines (A/B split) because of the 3×/week frequency — the prompt allows 1–3 routines in a single file. Both were emitted in one JSON block, which imported cleanly as "Added 10 exercises, 2 routines."
- A live copy of this conversation lives at <https://claude.ai/share/f45f4a2c-8be9-404c-bee2-e1fcfe57abc9> — kept here only as historical reference; the transcript above is the authoritative version since external share links can rot.
