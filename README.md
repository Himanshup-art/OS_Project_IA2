# Deadlock Analysis & Resource Allocation Dashboard

An interactive, presentation-ready web dashboard demonstrating **Banker's Algorithm for Deadlock Avoidance**, **Deadlock Detection with Wait-For Graphs**, **Deadlock Recovery via Process Termination**, and **Deadlock Prevention Policies** across multi-process, multi-resource operating system environments.

![Dashboard Preview](https://img.shields.io/badge/Status-Complete-success?style=for-the-badge)
![Tech Stack](https://img.shields.io/badge/Stack-HTML5%20%7C%20Vanilla%20CSS3%20%7C%20ES6%2B%20JavaScript-blue?style=for-the-badge)
![License](https://img.shields.io/badge/Course-Operating%20Systems%20(IA2)-purple?style=for-the-badge)

---

## 📌 Project Overview

This dashboard was developed for the **Operating Systems Course Activity 2 (Deadlock Analysis & Resource Allocation)**. It provides a visual and analytical laboratory for analyzing how modern operating systems manage resource requests, preserve safe states, detect circular dependencies, and enforce prevention constraints.

### Key Highlights
- **Zero Dependencies:** Pure vanilla HTML, CSS, and modern JavaScript. Requires no node servers, external libraries, or backend setups—simply open `index.html` in any browser.
- **Dynamic Configuration:** Supports 1 to 10 processes and 1 to 10 resource types with dynamic matrix generation.
- **Step-by-Step Visualization:** Visualizes every iteration of the Work vector and Need comparisons during safety evaluations.
- **Instant Presets:** One-click presets for classic benchmark scenarios (Safe State, Exceeds Available, Unsafe State, Deadlock).
- **Comprehensive Test Suite:** Evaluates Scenarios A through F and populates an analytical comparison table (TC1–TC6).

---

## 🔬 System Modules & Features

### 1. Banker's Algorithm (Deadlock Avoidance)
- Computes the remaining **Need Matrix** dynamically:
  $$\text{Need}[i][j] = \text{Maximum}[i][j] - \text{Allocation}[i][j]$$
- Implements Dijkstra's Banker's Algorithm safety check:
  - Initializes $\text{Work} = \text{Available}$ and $\text{Finish}[i] = \text{false}$.
  - Identifies processes where $\text{Need}_i \le \text{Work}$.
  - Simulates process completion and resource release: $\text{Work} = \text{Work} + \text{Allocation}_i$.
  - Generates the complete safe execution sequence (e.g., $\langle P_1 \to P_3 \to P_4 \to P_0 \to P_2 \rangle$).

### 2. Resource Request Handling
Simulates real-time process requests and enforces the 3-step Banker's validation:
1. **Validation 1:** $\text{Request}_i \le \text{Need}_i$ (Ensures process does not exceed declared maximum).
2. **Validation 2:** $\text{Request}_i \le \text{Available}$ (Checks if free resources currently exist).
3. **Validation 3 (Simulated Safety):** Temporarily allocates resources, tests if a safe sequence still exists, and grants or denies the request with full rollback.

### 3. Deadlock Detection & Recovery
- Operates independently of maximum claims using current allocation and outstanding requests.
- Constructs an active **Wait-For Graph (WFG)** to identify circular wait cycles.
- Demonstrates **Deadlock Recovery** via process termination:
  - Terminates a selected deadlocked process.
  - Reclaims its held resource instances back into the Available pool.
  - Re-evaluates system state to demonstrate resolution of the cycle.

### 4. Deadlock Prevention Strategies
Demonstrates how eliminating one of Coffman's four necessary conditions structurally prevents deadlock:
- **Eliminate Circular Wait:** Enforces a strict total resource ordering ($R_0 < R_1 < R_2 < \dots$). Processes can only request resources in strictly ascending order.
- **Eliminate Hold-and-Wait:** Implements all-or-nothing allocation where processes must request all required resources simultaneously prior to execution.
- **Trade-off Analysis:** Compares before/after matrices and details trade-offs in CPU concurrency and resource utilization.

### 5. Boundary & Invalid Input Validation (Scenario F)
Distinguishes valid boundary conditions from invalid inputs:
- **Zero Available Resources:** Valid edge case; safely handles zero free units.
- **Process with Zero Need:** Valid boundary case; process already holds all needed instances.
- **Invalid Process ID:** Out-of-bounds rejection with user feedback.
- **Negative Request Vector:** Input validation error; rejected immediately.
- **Allocation Exceeding Maximum Claim:** Violation of fundamental Banker's constraint; flagged and blocked.

---

## 📊 Analytical Results Table (TC1 – TC6)

The dashboard includes a built-in analytical comparison table matching the required evaluation criteria:

| Test Case | System Condition | Actual Dashboard Output | Expected Result | Analysis & Conclusion |
| :---: | :--- | :--- | :--- | :--- |
| **TC1** | Safe state (Scenario A) | Safe sequence found: $\langle P_1 \to P_3 \to P_4 \to P_0 \to P_2 \rangle$ | Safe sequence found | System is verified safe. Need $\le$ Work verified at each step. Work increments sequentially to completion. Deadlock impossible. |
| **TC2** | Request exceeds Available (Scenario B) | Request delayed; process placed in wait state | Request not granted immediately | Request is valid ($\le$ Need) but exceeds free instances. Allocation remains unchanged. This is normal resource contention, not a deadlock. |
| **TC3** | Request makes state unsafe (Scenario C) | Request denied; original allocation preserved | Request denied; state unchanged | Simulated allocation satisfies $\text{Request} \le \text{Available}$, but resulting state leaves no complete safe sequence. Banker's denies request and rolls back. |
| **TC4** | Deadlocked scenario (Scenario D) | Deadlock detected ($P_0, P_1, P_2$). Recovery: $P_0$ terminated | Deadlock identified by detection method | Wait-For Graph highlights active cycle. Terminating $P_0$ releases $[1, 0, 0]$, breaking the cycle and resolving deadlock. |
| **TC5** | Prevention policy applied (Scenario E) | Ordering $R_0 < R_1 < R_2$ & All-or-nothing applied | Selected deadlock condition eliminated | Structural elimination of Circular Wait or Hold-and-Wait. Eliminates deadlock risk at the cost of lower concurrency. |
| **TC6** | Boundary & invalid inputs (Scenario F) | Validation suite verified all 5 boundary/invalid cases | Correct handling or validation message | Correctly distinguishes valid edge cases (zero avail, zero need) from rejected invalid inputs (negative values, claim violations). |

---

## 🧠 Crucial Theoretical Distinctions

1. **Avoidance vs. Detection:**
   - **Banker's Algorithm (Avoidance):** Proactive. Evaluates *future* requests before granting them to ensure the system never leaves a safe state. Requires advance declaration of maximum demand.
   - **Deadlock Detection:** Reactive. Analyzes the *current* allocation state without maximum claims to see if processes are already blocked in an unresolvable cycle.
2. **Unsafe State vs. Deadlocked State:**
   - An **unsafe state is NOT necessarily a deadlock**. It means the system cannot *guarantee* that all processes will complete under worst-case maximum demand. Deadlock is a strict subset of unsafe states where processes are actively stuck.
3. **Circular Wait: Single vs. Multiple Resource Instances:**
   - For single-instance resources, a cycle in the Resource Allocation Graph is **necessary and sufficient** for deadlock.
   - For multiple-instance resources, a cycle is **necessary but NOT sufficient**, because processes outside the cycle may release needed instances.

---

## 🚀 How to Run the Project

No installation, runtime, or dependencies are required.

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Himanshup-art/OS_Project_IA2.git
   cd OS_Project_IA2
   ```
2. **Run the dashboard:**
   - Simply double-click `index.html` to open it in your web browser (Google Chrome, Microsoft Edge, Firefox, Safari).
   - Alternatively, open via terminal:
     ```powershell
     Start-Process index.html
     ```

---

## 📁 Repository Structure

```
OS_Project_IA2/
├── index.html                    # Core semantic HTML layout & accessible navigation
├── app.js                        # Complete OS algorithms, validation logic, & DOM engine
├── styles.css                    # Professional light-theme styling, CSS variables & print rules
└── README.md                     # Comprehensive project documentation & analytical summary
```

---

## 👥 Contributors

- **Aaditya Hande**
- **Swapnil Kamble**
- **Himanshu Patil**
- **Aditya Suryawanshi**
- **Chetan Gore**
- Course: Operating Systems (Course Activity 2 / IA2)
