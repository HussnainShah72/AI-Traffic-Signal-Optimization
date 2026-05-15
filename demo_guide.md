# 📖 Project Demonstration & Customization Guide

This guide provides step-by-step instructions for demonstrating the **AI Traffic Signal Optimization System** and explains how to customize the interface and logic for further experimentation.

---

## 🚀 How to Demonstrate

### 1. Basic Flow Visualization
- **Start the Simulation**: Enable the "Real-Time Simulation" checkbox in the sidebar.
- **Observe the Agents**: Watch how Intersections A and B coordinate. Note that the green light duration is dynamic based on traffic density.
- **Check the Logs**: Expand the "Multi-Agent Cooperative Decision Logs" to see the "Agent-to-Agent" communication.

### 2. Emergency Preemption
- Click **"Trigger Emergency (West to East)"**.
- Observe how Intersection A immediately switches its signal to clear the West lane.
- Notice the animated 🚑 ambulance icon and the siren effect.
- Once the vehicle clears Intersection A, watch Intersection B automatically prepare the West lane to receive the emergency vehicle.

### 3. Scenario Testing
- Switch the **Traffic Pattern** to "Rush Hour".
- Observe the exponential increase in vehicle inflow.
- Watch how the A* algorithm prioritizes high-congestion lanes to prevent total gridlock.

---

## 🎨 Customization Guide

### 1. Changing the UI Background
To modify the dashboard's theme, edit `src/config/styles.py`:
```python
# Change background color in .stApp class
.stApp { background-color: #0d1117; color: #f8fafc; }
```

### 2. Modifying Traffic Light Timings
The minimum green time is defined in `src/logic/simulation.py`:
```python
MIN_GREEN_TIME = 6  # Change this value to adjust signal stability
```

### 3. Customizing the A* Heuristic
You can adjust the "intelligence" of the agents in `src/logic/a_star.py`. For example, to make them even more cooperative:
```python
if neighbor_direction and state.current_phase == neighbor_direction:
    if neighbor_congestion > 15:
        h_cost += neighbor_congestion * 20  # Increase penalty for neighbor overflow
```

### 4. Adding Custom Buttons
To add new controls to the sidebar, modify the `with st.sidebar:` block in `app.py`:
```python
if st.button("Manual Reset"):
    st.session_state.cars_A = {"North": 0, "South": 0, "East": 0, "West": 0}
    st.rerun()
```

---

## 📈 Understanding the Benchmarks

When running the **Performance Comparison**:
- **AI Adaptive System**: Uses A* Search to find the mathematically optimal phase.
- **Traditional Fixed-Time**: Simulates a standard 6-second rotation cycle used in most cities.
- **Goal**: Show that the AI system consistently reduces "Average Wait Time" by 20-40% under variable traffic conditions.

---
*Created for the AI Traffic Signal Optimization Project.*
