# Feature Specification: User Progress Tracking

## Objective

Add a feature that allows users to track their speed and accuracy over time across selected Gym applications.

The feature will store quiz history locally in the browser and provide graphical progress visualization for each Gym separately.

---

# Scope

This feature applies only to the following Gym applications:

1. Gym Decimals
2. Functions Gym
3. DotProducts Gym
4. Fractions-add-gym
5. LinearEquations-Gym
6. Indices-Gym
7. Polynomials Gym

No server-side storage is required.

All data will be stored locally using browser Local Storage.

---

# Functional Requirements

## 1. Quiz History Recording

Only fully completed quizzes that reach the final results screen shall be recorded.

Partially completed or abandoned quizzes must NOT be stored.

Save Gym sessions automatically for all the following  gyms.
1. Gym Decimals
2. Functions Gym
3. DotProducts Gym
4. Fractions-add-gym
5. LinearEquations-Gym
6. Indices-Gym
7. Polynomials Gym

### Data to Store

Each history entry must contain:

| Field | Description |
|---------|---------|
| id | Unique session identifier (crypto.randomUUID preferred) |
| date | ISO date/time when quiz was completed |
| gymName | Exact Gym title reported by the Gym application |
| questionSummary | Number of Easy / Medium / Hard questions |
| totalQuestions | Total number of questions attempted |
| correctAnswers | Number of correctly answered questions |
| accuracy | Accuracy percentage |
| timeTakenSeconds | Total time taken to complete quiz |

### Example Record

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "date": "2026-05-26T10:30:00",
  "gymName": "Functions Gym",
  "questionSummary": {
    "easy": 8,
    "medium": 6,
    "hard": 4
  },
  "totalQuestions": 18,
  "correctAnswers": 16,
  "accuracy": 88.89,
  "timeTakenSeconds": 95
}
```

If totalQuestions is 0, the session shall NOT be saved.

---

# Storage Requirements

Store all history records in Local Storage.

Suggested key:

```text
gymQuizHistory_v1
```

Structure:

```json
[
  { quizRecord1 },
  { quizRecord2 },
  { quizRecord3 }
]
```

### Storage Order

New quiz results shall be inserted at the beginning of the history array.

```js
history.unshift(newRecord)
```

The most recent session always appears first.

### History Limit

Keep only the most recent 500 completed quiz sessions.

When a new record is added and the limit is exceeded:

1. Retain newest-first ordering.
2. Remove the oldest records.
3. Keep only the newest 500 sessions.

---

# 2. Home Page Integration

Add a new menu item:

```text
Track Progress
```

### Placement

Add "Track Progress" to the Hamburger menu in the top-right corner alongside Login / Logout options.

Selecting the menu item shall navigate to the Progress Tracking page.

---

# 3. Progress Tracking Page

Create a dedicated page for viewing historical performance. This should be full sized page.

The progress page shall use the exact Gym titles supplied by the Gym applications. No manual renaming or mapping shall be performed.

The progress tracking page should have the following sections in the same order (top to bottom)
1. Summary Statistics Card 
2. Gym Selection Dropdown
3. Accuracy Graph & Speed Graph (size by side OR one below the other)
4. Session History Table

## Page Layout

### Gym Selection Dropdown

Label:

```text
Select Gym to Plot
```

Populate the dropdown dynamically using the exact Gym names supported by the application.

---

## Summary Statistics Card

Display a summary section above the graphs.

Suggested fields:

```text
Total Sessions
Average Accuracy
Best Accuracy
Average Correct Answers Per Minute
```

---

## Data Loading

When a Gym is selected:

1. Load all history records for that Gym.
2. Sort records chronologically for graph plotting.
3. Generate performance charts.
4. Display summary statistics.
5. Display session history table.

---

# 4. Accuracy Graph

Label X axis and y Axis clearly in large visible text
Use tick intervals of 10%.
Display range from 0% to 100%.

Plot:

Accuracy (%) vs Session Number

### X Axis

Session Number

History records shall be sorted chronologically before plotting.

The first recorded session shall be displayed as:

1

The second recorded session shall be displayed as:

2

The third recorded session shall be displayed as:

3

and so on.

The X-axis shall therefore represent the student's progression over time rather than calendar dates.

### Y Axis

Accuracy percentage.

Accuracy shall be calculated as:

```text
(correctAnswers / totalQuestions) × 100
```

Display accuracy rounded to 1 decimal place.

Example:

```text
88.9%
```

---

# 5. Speed Graph

Label X axis and Y axis clearly in large visible text.

Metric:

Correct Answers Per Minute

Plot:

Correct Answers Per Minute vs Date

### X Axis

Session Number

History records shall be sorted chronologically before plotting.

The first recorded session shall be displayed as:

1

The second recorded session shall be displayed as:

2

The third recorded session shall be displayed as:

3

and so on.

The X-axis shall therefore represent the student's progression over time rather than calendar dates.

### Y Axis

Correct Answers Per Minute.

Calculation:

(correctAnswers × 60) / timeTakenSeconds

Higher values indicate faster performance.

Display values rounded to 1 decimal place.

Example:

12.4 questions/minute

### Y-Axis Scaling

The Y-axis shall be automatically scaled based on the actual values being plotted so that improvements and declines in speed are clearly visible.

Requirements:

- Determine minimum and maximum values from the plotted data.
- Add approximately 10–15% padding above and below the observed range.
- Enforce a minimum visible range of 5 questions/minute.
- Use human-friendly tick intervals whenever possible.
- Do not force the Y-axis to start at zero.

If only a single data point exists, display a sensible range around the value so that the graph remains readable.

# 6. Graph Library

Use Recharts for graph rendering.

Both graphs shall support:

- Responsive layout
- Tooltips
- Automatic scaling
- Date-based X-axis rendering

---
Interactive Data Point Details

Both graphs shall support clicking on a data point.

When a data point is clicked, display a tooltip or popup containing:

Session Number
Date and Time
Correct Answers
Total Questions
Accuracy (%)
Correct Answers Per Minute

Example:

Session #12

26-May-2026 10:30 AM

Correct Answers: 16 / 18

Accuracy: 88.9%

Speed: 10.1 questions/minute

The tooltip shall remain visible until dismissed or another data point is selected.

# 7. Session History Table

Display a table below the graphs.

Suggested columns:

| Date | Easy | Medium | Hard | Total Questions | Correct Answers | Accuracy (%) | Correct Answers/Minute |
|--------|--------|--------|--------|--------|--------|--------|--------|

### Date Display Format

Display dates in a consistent user-friendly format.

Suggested format:

```text
26-May-2026 10:30 AM
```

### Table Sorting

Display newest sessions first.

Order:

```text
Most Recent
↓
Older
↓
Oldest
```

The same ordering shall be used across paginated views.

### Pagination

If the number of sessions exceeds the page size, paginate the table.

Default page size:

```text
20 sessions per page
```

Provide:

- Previous Page button
- Next Page button
- Current page indicator

---

# 8. No History Handling

If no records exist for the selected Gym, display:

```text
No progress history available for this Gym yet.
Complete a quiz to start tracking progress.
```

Additionally:

- Do not render graphs.
- Do not render the session table.
- Do not render pagination controls.

---

# User Flow

Home Page
→ Hamburger Menu
→ Track Progress
→ Select Gym
→ Load History
→ Display Summary Statistics
→ Display Accuracy Graph
→ Display Speed Graph
→ Display Session History Table

---

# Technical Notes

## Local Storage Only

- No login system required.
- No user profiles required.
- Data remains available only on the current browser/device.
- Clearing browser data removes stored history.

## Performance Considerations

- Load history only when the Progress page is opened.
- Filter records by Gym before plotting.
- Sort chronologically before graph rendering.
- Enforce the 500-session limit whenever a new session is saved.
- Avoid unnecessary re-rendering of graphs.

---

# Future Enhancements (Out of Scope)

- Cloud synchronization
- User accounts
- Login/password support
- Multi-device tracking
- Exporting history
- Comparing multiple Gyms on the same graph
- Leaderboards
