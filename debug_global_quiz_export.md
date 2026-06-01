# Global Quiz Session Debug Export — Specification

# Overview

Add a lightweight debug/export feature across ALL quiz topics and modes.

This feature should work universally across all topics including:

- Arithmetic
- Percentages
- Algebra
- Rapid Fire
- Fraction quizzes
- Polynomial quizzes
- Future quiz modes

The purpose of this feature is to:

- capture complete quiz sessions
- analyze gameplay behavior
- debug adaptive difficulty
- inspect generated questions
- track scoring behavior
- review user mistakes
- support future analytics work

This feature should work entirely locally.

No backend or cloud storage is required.

---

# Production Safety Requirement

This feature is intended ONLY for development/debug builds.

The export system MUST NOT be enabled in production builds by default.

The implementation should use:

Environment-based build configuration

Recommended behavior:

| Environment | Export Behavior |
|---|---|
| Development | Enabled |
| Production | Disabled |

The export logic should automatically detect the build environment.

No manual toggling should be required during normal development.

---

# Scope

This feature should be implemented globally for all quiz systems.

The export mechanism should be reusable and shared across quiz modes.

Avoid mode-specific implementations.

---

# Trigger Conditions

The session export should happen automatically when:

1. a quiz/session ends naturally
OR
2. the user manually presses Stop/Finish/Quit

Both flows should generate a session export file.

---

# Export Format

Use:

JSON

Reason:
- human readable
- easy to debug
- easy to analyze later
- easy to reload into tools/scripts
- future-proof

---

# File Storage Behavior

The export should be downloaded locally to the user's machine.

No server upload required.

No login/account system required.

---

# Export File Naming

Use timestamp-based file naming.

Requirements:

- unique per session
- human readable
- sortable chronologically

Example:

quiz-session-2026-05-19T08-22-10Z.json

Optional:
- include quiz mode name in filename

Example:

percentages-2026-05-19T08-22-10Z.json

---

# Exported Session Structure

The exported JSON file should contain:

1. Session metadata
2. Full per-question history

---

# Session Metadata Requirements

Store:

- quiz mode name
- session timestamp
- final score
- total questions answered
- total correct answers
- total wrong answers
- accuracy percentage
- total duration played

Optional fields:
- highest difficulty reached
- average response time
- adaptive mode enabled/disabled
- difficulty label

---

# Per Question Data Requirements

For EVERY question answered, store:

- question index
- question type
- question prompt
- user answer
- correct answer
- correctness
- difficulty value
- points earned
- time spent

Optional:
- generator name
- adaptive difficulty level
- hint used
- solved manually vs auto reveal

---

# Difficulty Logging

If the quiz mode supports adaptive difficulty:

store the raw floating-point difficulty value.

Examples:

0.21
0.53
0.81

This is important for:
- balancing
- analytics
- adaptive difficulty debugging

If a quiz mode does NOT use adaptive difficulty:

store:
- fixed difficulty label
OR
- null

---

# Timing Information

Store the response time for every question.

Examples:

2.1 seconds
5.7 seconds
11.2 seconds

This will help identify:
- frustration
- difficult generators
- slow question types

---

# Incorrect Answers

Wrong answers should still be fully logged.

Requirements:

- include user answer
- include correct answer
- include time spent
- include difficulty
- include points earned

If the mode does not use scoring:
- points may be null or 0

---

# Correct Answers

Correct answers should include:

- earned points
- difficulty
- time spent

---

# Results Table Synchronization

The exported data should match the same information displayed in the on-screen results table.

The export and UI should remain consistent.

---

# Export Timing

The export should happen immediately after:
- quiz completion
OR
- Stop/Quit button press

The user should receive the downloaded file automatically.

No additional confirmation dialog required.

---

# Performance Requirements

The export feature should:

- be lightweight
- avoid blocking the UI
- avoid noticeable lag
- work entirely client-side

---

# Architecture Notes

The implementation should reuse existing:

- results arrays
- score state
- timer state
- adaptive difficulty state
- quiz metadata state

The goal is:
- minimal code changes
- minimal architectural disruption
- shared reusable export utility

---

# Recommended Architecture

Create a shared export utility/helper.

All quiz modes should call the same export mechanism.

Avoid:
- duplicate export logic
- per-topic implementations
- custom serializers per quiz

The export system should remain generic.

---

# Security / Privacy Notes

The exported file remains entirely local.

No network calls should be made.

No data collection should occur.

No analytics service should be integrated.

---

# MVP Scope

Version 1 should include ONLY:

- automatic local JSON export
- export on Finish
- export on Stop/Quit
- full per-question logging
- session metadata

---

# Explicitly Excluded

Do NOT implement yet:

- cloud sync
- backend APIs
- database storage
- leaderboard upload
- user accounts
- replay viewer
- analytics dashboard
- compression
- encryption
- remote telemetry

---

# Design Goal

The purpose of this feature is to create a complete debug snapshot of every quiz session.

The exported data should make it easy to:

- inspect generated questions
- debug scoring
- debug adaptive difficulty
- analyze user behavior
- tune question generators
- review edge cases
- identify problematic questions

without requiring any backend infrastructure.

---

# Long-Term Benefits

This feature will later enable:

- balancing quiz difficulty
- identifying bad generators
- identifying confusing questions
- student progress analytics
- replay systems
- leaderboard systems
- adaptive learning improvements

without needing major architectural changes later.
