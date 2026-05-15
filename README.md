# 🚦 Multi-Agent AI Traffic Signal Optimization System

**An intelligent, cooperative traffic management simulator powered by A* Search and Multi-Agent coordination.**

<div align="center">

[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.32+-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)](https://streamlit.io)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square)
![Type](https://img.shields.io/badge/Type-Academic_Project-blue?style=flat-square)
![Domain](https://img.shields.io/badge/Domain-Artificial_Intelligence-orange?style=flat-square)

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

This project implements a **real-time, multi-agent traffic signal control system** that uses **A* Search** with domain-specific heuristics to dynamically optimize traffic flow across a network of interconnected intersections. Unlike traditional fixed-time controllers, this system enables two autonomous agents (one per intersection) to **cooperatively communicate** and adjust their signal phases based on live traffic conditions, emergency scenarios, and neighbor congestion levels.

Built as a fully interactive **Streamlit** web dashboard, the system provides real-time visualization of traffic dynamics, signal state transitions, and cooperative agent decision logs.

---

## ✨ Key Features

| Feature | Description |
|:--- |:--- |
| **A* Search Optimization** | Each agent uses A* pathfinding with a custom heuristic to select the optimal signal phase every cycle. |
| **Multi-Agent Cooperation** | Two intersection agents share congestion data and penalize actions that would overflow a congested neighbor. |
| **Emergency Preemption** | Dedicated emergency routing (West ↔ East) with signal preemption and animated ambulance visualization. |
| **Scenario Simulation** | Supports Normal, Rush Hour, and Night traffic patterns with configurable inflow intensity. |
| **Real-Time Visualization** | Interactive intersection grid with animated traffic lights, crosswalks, vehicle counters, and siren effects. |
| **Performance Benchmarking** | Side-by-side comparison of A* Adaptive vs. Traditional Fixed-Time controllers with charts and raw metrics. |
| **Cooperative Decision Logs** | Timestamped logs showing each agent's decisions and cooperative interventions in real time. |

---

## 🏗 System Architecture

```text
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

Each intersection operates as an **independent agent** that receives its neighbor's total congestion count every simulation cycle. The A* heuristic incorporates this neighbor data to **penalize phase selections** that would route cleared vehicles toward an already-congested neighbor — enabling emergent cooperative behavior without centralized control.

---

## 🧠 AI Algorithms

### A* Search with Domain-Specific Heuristic

The core decision engine (`a_star.py`) evaluates all possible signal phases and selects the one with the lowest combined cost:

`f(phase) = g(phase) + h(phase)`

- **g(phase)** (Path cost): Remaining vehicles across all lanes after clearing the selected phase + switch penalty (+8) if changing phase.
- **h(phase)** (Heuristic cost): 
  - Exponential penalty for waiting vehicles: `count^1.5 * 5`.
  - Scenario multipliers (e.g., Night: x2.0).
  - Emergency preemption: Extreme penalty for non-priority lanes.
  - **Cooperation**: Penalty proportional to neighbor congestion if routing traffic toward them.

---

## 🛠 Tech Stack

- **Python 3.8+**: Core programming language
- **Streamlit**: Interactive web dashboard
- **Pandas**: Data manipulation for analytics
- **heapq**: Priority queue for A* search

---

## 🚀 Getting Started

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/HussnainShah72/AI-Traffic-Signal-Optimization.git
   cd AI-Traffic-Signal-Optimization
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the application**
   ```bash
   streamlit run app.py
   ```

---

## 📁 Project Structure

- `app.py`: Main Streamlit application
- `src/logic/a_star.py`: A* Search implementation
- `src/logic/simulation.py`: Traffic simulation engine
- `src/analytics/comparison.py`: Performance benchmarking
- `src/ui/components.py`: UI rendering logic
- `src/config/styles.py`: Custom CSS and themes

---

## 📄 License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Built for Artificial Intelligence Project Coursework**

</div>
