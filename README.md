<![CDATA[<div align="center">

# 🚦 Multi-Agent AI Traffic Signal Optimization System

**An intelligent, cooperative traffic management simulator powered by A\* Search and Multi-Agent coordination.**

[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.32+-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)](https://streamlit.io)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

<br/>

<img src="https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square" alt="Status"/>
<img src="https://img.shields.io/badge/Type-Academic_Project-blue?style=flat-square" alt="Type"/>
<img src="https://img.shields.io/badge/Domain-Artificial_Intelligence-orange?style=flat-square" alt="Domain"/>

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [System Architecture](#-system-architecture)
- [AI Algorithms](#-ai-algorithms)
- [Tech Stack](#-tech-stack)
- [Getting Started](#-getting-started)
- [Project Structure](#-project-structure)
- [Usage Guide](#-usage-guide)
- [Performance Analysis](#-performance-analysis)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🔍 Overview

This project implements a **real-time, multi-agent traffic signal control system** that uses **A\* Search** with domain-specific heuristics to dynamically optimize traffic flow across a network of interconnected intersections. Unlike traditional fixed-time controllers, this system enables two autonomous agents (one per intersection) to **cooperatively communicate** and adjust their signal phases based on live traffic conditions, emergency scenarios, and neighbor congestion levels.

Built as a fully interactive **Streamlit** web dashboard, the system provides real-time visualization of traffic dynamics, signal state transitions, and cooperative agent decision logs.

---

## ✨ Key Features

| Feature | Description |
|---|---|
| **A\* Search Optimization** | Each agent uses A\* pathfinding with a custom heuristic to select the optimal signal phase every cycle. |
| **Multi-Agent Cooperation** | Two intersection agents share congestion data and penalize actions that would overflow a congested neighbor. |
| **Emergency Vehicle Preemption** | Dedicated emergency routing (West ↔ East) with signal preemption and animated ambulance visualization. |
| **Scenario Simulation** | Supports Normal, Rush Hour, and Night traffic patterns with configurable inflow intensity. |
| **Real-Time Visualization** | Interactive intersection grid with animated traffic lights, crosswalks, vehicle counters, and siren effects. |
| **Performance Benchmarking** | Side-by-side comparison of A\* Adaptive vs. Traditional Fixed-Time controllers with charts and raw metrics. |
| **Cooperative Decision Logs** | Timestamped logs showing each agent's decisions and cooperative interventions in real time. |

---

## 🏗 System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Streamlit Dashboard                   │
│   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│   │   Sidebar     │  │ Intersection │  │ Intersection │  │
│   │  (Controls)   │  │    Agent A   │  │    Agent B   │  │
│   └──────────────┘  └──────┬───────┘  └──────┬───────┘  │
│                            │                  │          │
│                     ┌──────▼──────────────────▼──────┐   │
│                     │   Cooperative Message Exchange  │   │
│                     │   (Congestion Data Sharing)     │   │
│                     └──────────────┬─────────────────┘   │
│                                   │                      │
│              ┌────────────────────▼──────────────────┐   │
│              │        A* Search Engine               │   │
│              │  ┌──────────┐  ┌──────────────────┐   │   │
│              │  │ Heuristic│  │ State Expansion  │   │   │
│              │  │ Function │  │  & Cost Calc     │   │   │
│              │  └──────────┘  └──────────────────┘   │   │
│              └───────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### Agent Communication Model

Each intersection operates as an **independent agent** that receives its neighbor's total congestion count every simulation cycle. The A\* heuristic incorporates this neighbor data to **penalize phase selections** that would route cleared vehicles toward an already-congested neighbor — enabling emergent cooperative behavior without centralized control.

---

## 🧠 AI Algorithms

### A\* Search with Domain-Specific Heuristic

The core decision engine (`a_star.py`) evaluates all possible signal phases and selects the one with the lowest combined cost:

```
f(phase) = g(phase) + h(phase)
```

**g(phase)** — Path cost:
- Remaining vehicles across all lanes after clearing the selected phase
- Switch penalty (+8) if the phase differs from the current one

**h(phase)** — Heuristic cost:
- Exponential penalty for waiting vehicles on non-active lanes: `count^1.5 × 5`
- Scenario multipliers (Rush Hour: ×1.2, Night: ×2.0)
- Emergency preemption priority: +100,000 penalty for non-emergency phases
- **Cooperative penalty**: +`neighbor_congestion × 10` if the selected phase would send traffic toward a congested neighbor (>15 vehicles)

### Decision Policies

| Condition | Agent Behavior |
|---|---|
| Emergency active | Immediate preemption to emergency lane |
| Timer < MIN_GREEN (6 cycles) | Maintain current green (stability) |
| A\* recommends same phase | Continue clearing (extend green) |
| A\* recommends different phase | Switch signal (yellow transition) |

---

## 🛠 Tech Stack

| Technology | Purpose |
|---|---|
| **Python 3.8+** | Core programming language |
| **Streamlit** | Interactive web-based dashboard and UI rendering |
| **Pandas** | Data manipulation for performance comparison charts |
| **heapq** | Priority queue implementation for A\* search |

---

## 🚀 Getting Started

### Prerequisites

- Python **3.8** or higher
- pip (Python package manager)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/AI-Traffic-Signal-Optimization.git
   cd AI-Traffic-Signal-Optimization
   ```

2. **Create a virtual environment** *(recommended)*
   ```bash
   python -m venv venv
   source venv/bin/activate        # Linux / macOS
   venv\Scripts\activate           # Windows
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run the application**
   ```bash
   streamlit run app.py
   ```

5. Open your browser at **`http://localhost:8501`** to access the dashboard.

---

## 📁 Project Structure

```
AI_Project/
│
├── app.py                        # Main Streamlit application entry point
├── requirements.txt              # Python dependencies
├── README.md                     # Project documentation
│
└── src/                          # Source package
    ├── __init__.py
    │
    ├── config/                   # Configuration & theming
    │   ├── __init__.py
    │   └── styles.py             # Custom CSS and page configuration
    │
    ├── logic/                    # Core AI & simulation engine
    │   ├── __init__.py
    │   ├── a_star.py             # A* Search algorithm with cooperative heuristic
    │   └── simulation.py         # Traffic simulation engine (inflow, clearing, logging)
    │
    ├── analytics/                # Performance analysis module
    │   ├── __init__.py
    │   └── comparison.py         # AI vs Fixed-Time benchmark comparison
    │
    └── ui/                       # Frontend rendering
        ├── __init__.py
        └── components.py         # Intersection grid, traffic lights, vehicle rendering
```

---

## 📖 Usage Guide

### Dashboard Controls

| Control | Function |
|---|---|
| **Traffic Pattern** | Select scenario: Normal, Rush Hour, or Night |
| **Inflow Intensity** | Adjust vehicle arrival rate (0–10) |
| **NEXT SIGNAL CYCLE** | Manually advance one simulation tick |
| **Start Real-Time Simulation** | Enable continuous auto-advance mode |
| **Trigger Emergency** | Dispatch an emergency vehicle (West→East or East→West) |
| **Run Comparison** | Execute AI vs. Fixed-Time benchmark with configurable cycle count |

### Interpreting the Dashboard

- **Green light** — Active phase (vehicles being cleared)
- **Yellow light** — Transition phase (signal switching)
- **Red light** — Stopped (vehicles queueing)
- **🚑 Siren animation** — Emergency vehicle in the lane
- **Vehicle count badges** — Real-time queue length per direction
- **Decision logs** — Agent reasoning with cooperative warnings for high congestion

---

## 📊 Performance Analysis

The built-in benchmarking module runs a controlled simulation comparing:

| Metric | Description |
|---|---|
| **Average Waiting Cars/Tick** | Mean number of vehicles in queue per cycle |
| **Total Cars Cleared** | Cumulative throughput over the simulation |
| **Max Congestion Peak** | Worst-case queue length observed |
| **Signal Phase Switches** | Number of signal transitions (lower = more stable) |

Results are rendered as **line charts** (congestion over time), **bar charts** (throughput comparison), and a **raw data table** for detailed analysis.

---

## 🤝 Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m "Add your feature"`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Built with ❤️ for Artificial Intelligence coursework**

</div>
]]>
