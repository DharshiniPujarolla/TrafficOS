# TrafficOS — Traffic Signal Controller Using Operating System Scheduling Algorithms

**Operating Systems × DSA Mini Project**

TrafficOS is an interactive traffic-signal simulator that maps CPU scheduling algorithms and core
Operating System concepts onto a physical 4-way intersection. Each traffic lane is modeled as a
process ready queue; the traffic signal is the CPU; the traffic controller is the scheduler.

## Setup & Run Instructions

**Requirements:** Node.js 18+ and npm.

1. Install dependencies:
   ```bash
   npm install
   ```

2. Start the local dev server:
   ```bash
   npm run dev
   ```
   Vite will print a local URL (typically `http://localhost:5173`) — open it in your browser.

3. Build for production (outputs to `dist/`):
   ```bash
   npm run build
   ```

4. Preview the production build locally:
   ```bash
   npm run preview
   ```

---

## What's in the App

Opening the app lands on a marketing/landing page describing the project. Click
**"Launch Interactive Simulator"** to open the simulation dashboard.

### Scheduling Algorithms
Four CPU scheduling algorithms are implemented and selectable from the dashboard:

| Algorithm | Type | What it does |
|---|---|---|
| First-Come, First-Served (FCFS) | Non-preemptive | Dispatches the ready queue whose head process arrived earliest. Runs to completion before re-dispatching. |
| Shortest Job First (SJF) | Non-preemptive | Dispatches the ready queue whose head process has the shortest next CPU burst. Runs to completion before re-dispatching. |
| Round Robin (RR) | Preemptive | Fixed time-quantum rotation across lanes via a circular queue. |
| Priority + Dynamic Aging | Preemptive | Min-heap-style priority dispatch; waiting processes age upward in priority over time to prevent starvation. |

A **Compare Mode** runs Round Robin and Priority+Aging side by side on the same input for direct
comparison of wait time, turnaround time, and context switches.

### OS Concepts Demonstrated

**Process & CPU Scheduling**
- Process states (running / ready / waiting), ready queues, context switching
- Circular queue (Round Robin) and priority queue (min-heap) data structures
- Dynamic aging and starvation detection/alerts

**Synchronization**
- **Mutual exclusion / critical section / semaphore** — the intersection center is modeled as a
  shared resource guarded by a binary semaphore; only one lane holds the lock at a time. Every
  context switch logs an explicit `wait(S)` / `signal(S)` pair. See the *Critical Section /
  Semaphore Monitor* panel.

**Deadlock**
- A dedicated **Deadlock Lab** panel: trigger a 4-lane circular wait-for graph
  (`NORTH -> EAST -> SOUTH -> WEST -> NORTH`), run real DFS-based cycle detection over the graph,
  then force a recovery that preempts one lane's lock to break the cycle. The wait-for graph is
  drawn live and highlights the detected cycle.

**Memory Management & Producer-Consumer**
- Each lane is a bounded buffer with a fixed capacity (6 slots). "Inject Process Thread" is the
  producer — it blocks once a lane is full. The scheduler completing a burst is the consumer — it
  frees a slot. See the *Memory Management — Bounded Buffers* panel.

**Data Structures & Algorithms**
- **Graph** — the intersection is modeled as a conflict graph (which lane pairs can never be
  green simultaneously). A greedy graph-coloring algorithm partitions the 4 lanes into
  non-conflicting signal-phase groups. See the *Conflict Graph* panel.
- **Hash Map** — every queued vehicle is indexed by ID in a `Map` for O(1) average-case lookup
  (vs. an O(n) scan across all 4 lanes). See the *Vehicle Registry* panel's search box.
- Queues, priority queues, and sorting underlie the scheduling logic throughout.

### Guided Demo Scenarios
Four preloaded scenarios illustrate specific concepts: Normal Traffic (fair RR slicing), Heavy
North Traffic (queue bottleneck), Emergency Vehicle (priority preemption), and Starvation
Demonstration (priority scheduling without aging).

---

## Project Structure

```
src/
  App.jsx                    # Entry component (renders TrafficOSSimulator)
  main.jsx                   # React root
  TrafficOSSimulator.tsx     # Everything: router, landing page, simulation dashboard,
                              # all scheduling/OS-concept panels, and visualizers
  components/                # Unused scaffold - not imported anywhere; kept for
                              # reference, safe to ignore or delete
  hooks/useInView.js         # Unused scaffold, same as above
```

Note: `TrafficOSSimulator.tsx` is a self-contained default export (`App`) that includes its own
hash-based router (`#/` = landing, `#/simulator` = dashboard). The files under `src/components/`
and `src/hooks/` are leftover scaffolding not wired into the app.

## Tech Stack
React 18 + Vite + Tailwind CSS. No backend — everything runs client-side in the browser.