# F1 Data Analytics App

> Project Definition & Long-Term Context

---

## 1. Project Mission

The goal of this project is to build a structured, modular analytics application for Formula 1 race weekend data.

The application will:

- Collect session data (practice, qualifying, race)
- Clean and structure lap-level data
- Add contextual labels (stints, tire usage, dirty air, etc.)
- Generate analytical insights
- Eventually expose those insights through an API and interactive frontend

**This is not just a visualization project — it is a structured analytics pipeline.**

---

## 2. Core Philosophy

This project follows a three-layer structure:

### 1️⃣ Exploration Layer (Notebooks)

Used to:

- Understand FastF1 data structure
- Experiment with filtering logic
- Validate statistical approaches
- Test new ideas

Notebooks are for:

- Thinking
- Iterating
- Prototyping

**They are not production code.**

### 2️⃣ Analytics Layer (Reusable Python Modules)

Located in: `F1_Data_Analysis/`

This layer:

- Contains stable, reusable logic
- Does not print or plot
- Returns clean DataFrames or structured objects
- Can be used by APIs, scripts, or ML models

**This is the core of the project.**

### 3️⃣ Application Layer (Future)

Planned:

- FastAPI backend
- Interactive frontend
- Predictive modeling components

This layer will consume the analytics layer.
It should contain **no analysis logic**, only orchestration.

---

## 3. Current Scope (Phase 1: Core Analytics)

The current focus is building reliable lap-level analytics.

### Immediate Goals

- Fetch and cache session data
- Filter valid race laps
- Calculate lap time summaries
- Detect stints and tire usage
- Label laps with contextual metadata (e.g., dirty air)

> No frontend work until analytics are stable.

---

## 4. Key Analytical Components

### 4.1 Session Handling

Responsible for:

- Loading sessions
- Managing caching
- Standardizing access to session metadata

Output:

- FastF1 session object
- Clean accessors for laps, results, weather

### 4.2 Lap Cleaning

Purpose:

- Remove in-laps and out-laps
- Remove safety car and virtual safety car laps
- Optionally remove yellow-flag laps
- Preserve only competitive green-flag laps

This will rely primarily on:

- `IsAccurate`
- Flag status
- Lap timing integrity

### 4.3 Race Pace Metrics

Core metrics to define:

- Best lap
- Median race pace
- Mean race pace
- Rolling average pace
- Pace delta vs session best
- Pace delta vs teammate

Race pace must:

- Preserve dirty-air laps
- Allow grouping by stint
- Allow grouping by tire compound

### 4.4 Stint and Tire Analysis

Per stint:

- Length
- Average pace
- Degradation rate
- Compound usage

Pit strategy summaries:

- Pit stop lap numbers
- Undercut/overcut potential
- Relative position change

### 4.5 Dirty Air Detection (Context Tagging)

Dirty air will **not** be excluded.

Instead, laps will be labeled using gap-to-car-ahead heuristics.

Each lap should include:

- `GapAhead`
- `DirtyAirLevel` (clean / moderate / heavy)

This allows:

- Clean-air pace comparison
- Dirty-air degradation analysis
- Improved predictive modeling later

### 4.6 Qualifying Analysis

- Best sector times
- Ideal lap construction
- Sector consistency
- Delta between ideal and actual best

---

## 5. Data Pipeline Design

The long-term flow:

```
FastF1 / Ergast
        ↓
Session Loader
        ↓
Lap Cleaning
        ↓
Context Enrichment (stints, dirty air, tires)
        ↓
Metric Computation
        ↓
API / Frontend / ML
```

Each stage should be **modular and testable**.

---

## 6. Folder Structure (Target Structure)

```
f1-analytics/
│
├── cache/
│
├── notebooks/
│   ├── 00_fastf1_sandbox.ipynb
│   ├── 01_session_overview.ipynb
│   ├── 02_lap_times_and_pace.ipynb
│   ├── 03_stints_and_tires.ipynb
│   ├── 04_qualifying_analysis.ipynb
│   └── 05_race_analysis.ipynb
│
├── F1_Data_Analysis/
│   ├── __init__.py
│   ├── sessions.py
│   ├── laps.py
│   ├── pace.py
│   ├── strategy.py
│   └── context.py
│
├── requirements.txt
└── PROJECT_CONTEXT.md
```

- Notebooks experiment.
- `F1_Data_Analysis/` defines the system.

---

## 7. What This Project Is NOT

- Not a pure visualization toy
- Not a single notebook analysis
- Not a script-only pipeline
- Not an ML-first project

**Machine learning comes later.**
**Clean structured analytics comes first.**

---

## 8. Future Phases

### Phase 2 – API Layer

- FastAPI endpoints
- Query by session, driver, stint
- JSON outputs

### Phase 3 – Frontend

- Interactive lap charts
- Dirty air highlighting
- Strategy visualization

### Phase 4 – Predictive Models

- Practice-to-race pace modeling
- Tire degradation prediction
- Strategy simulation

---

## 9. Long-Term Vision

The end state of this project should:

- Allow loading any F1 session
- Automatically generate structured race analytics
- Provide contextual insight (not just lap times)
- Serve as a foundation for modeling and visualization

The core strength of this project will be:

> **Structured lap-level context enrichment.**
>
> Everything else builds on that.

---

## 10. Guiding Rule to Avoid Context Rot

Whenever adding a feature, ask:

| Question | Destination |
|---|---|
| Is this exploration? | Notebook |
| Is this reusable logic? | `F1_Data_Analysis` module |
| Is this presentation? | API or frontend |
| Am I mixing layers? | Refactor |

- If something feels messy, it probably belongs in a **notebook**.
- If something feels stable, it belongs in a **module**.