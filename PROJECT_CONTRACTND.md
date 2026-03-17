# Project Contract (Extra Credit Research Track)

> This document is your binding agreement for Final Waiver and Automatic A eligibility.
> If the contract requirements are met by the deadlines, the outcome is guaranteed.

**Team Name:**  
**Members:** (Kevin Pol,Kevin_Pol@student.uml.edu, BLE setup + validation) (Andrew Thach, Andrew_Thach@student.uml.edu, experiment execution + data logging) (Odey Khello, Odey_Khello@student.uml.edu, analysis + plots + documentation)
**Survey Topic:** (Bluetooth/Wireless Interference and Protocol Performance)  
**Project Title:** Impact of WiFi Interference on Bluetooth Latency and Packet Loss
**GitHub Repo URL:**  https://github.com/PolKevin03/NDResearchProject.git
**Project Category:** (Measurement)

---

## 1) Survey: open question bridge
### 1.1 Three claims from your survey sources
Write 3 claim statements you found, each with a citation.

1. Claim: Bluetooth Low Energy performance degrades under interference, with measurable effects on reliability and communication quality in noisy wireless environments.
   - Source: “Bluetooth Low Energy Interference Awareness Scheme and Improved Channel Selection Algorithm for Connection Robustness” (2021)https://www.mdpi.com/1424-8220/21/7/2257
2. Claim:BLE implements adaptive frequency hopping to reduce interference, but performance degradation still occurs in environments with overlapping wireless systems.
   - Source: “Performance Evaluation of Bluetooth Low Energy: A Systematic Review” (2017) https://www.mdpi.com/1424-8220/17/12/2898 
3. Claim:WiFi and Bluetooth coexistence in the 2.4 GHz band leads to increased latency, packet loss, and reduced communication reliability due to channel overlap and contention.
   - Source: 
   “A Performance Study of Bluetooth Low Energy and WiFi Coexistence” (2021) https://ieeexplore.ieee.org/abstract/document/9345670

### 1.2 Your open question
Rewrite one claim into a measurable question:
- **Research question (1 sentence):**How does WiFi interference affect Bluetooth latency and packet loss under controlled conditions?
- **Hypothesis (falsifiable, 1 sentence):**Bluetooth latency and packet loss will increase when 2.4 GHz WiFi traffic is active, and trials with interference will show higher median/p95 latency and loss than trials without interference.

### 1.3 Difference from prior work
3–5 bullets that make your project unique. Examples:
- new condition sweep not covered in the closest paper(s)
- new metric (tail latency, fairness, variance)
- new implementation/config comparison
- replication + extension on one missing dimension

- Uses a real hardware laptop-to-ESP32 measurement setup rather than simulation-based analysis
- Measures both packet loss and latency tail metrics, which are not always jointly evaluated in prior work
- Implements a reproducible experiment pipeline with automated CSV logging and plot generation
- Focuses on a controlled experimental setup (fixed distance, repeated trials) to isolate interference effects
- Compares Bluetooth performance between no-WiFi baseline and active 2.4 GHz WiFi interference conditions

---

## 2) Project scope
### 2.1 In scope (what you will do)
Bullet list of concrete tasks.
- Set up the ESP32 to receive and respond to Bluetooth messages
- Set up the laptop to send messages and measure response time
- Send “ping” messages and measure how long it takes to get a reply (latency)
- Count a packet as lost if no reply is received within a set time
- Create WiFi traffic on 2.4 GHz to act as interference
- Run multiple trials for each condition (with and without WiFi)
- Save all results into CSV files
- Use the CSV data to generate graphs (latency and packet loss)
- Document how everything works in the GitHub README

### 2.2 Out of scope (what you will NOT do)
This is critical for fairness. List what you are explicitly not attempting.
- Analyze low-level radio signals (PHY layer)
- Test multiple Bluetooth devices at once
- Measure power or energy usage
- Study Bluetooth security features
- Design new interference-handling algorithms
- Test across many locations or moving setups
---

## 3) Deliverables
To qualify for final waiver + automatic A, you must deliver **all** items below.
As of right now the Project Contract

### 3.1 Repo deliverable
Your repo must include, at minimum:

```
src/
scripts/
docs/
results/
README.md
```

### 3.2 Reproducibility deliverable
Provide exact commands that regenerate your results:

- **Run experiments** (must produce CSV in `results/`):
```bash
python scripts/run_experiment.py
```

- **Generate plots** (must produce figures in `results/`):
```bash
python scripts/analyze_data.py
```

### 3.3 Demo video
- Private YouTube link: To be completed at final stage (hardware pending)
- Timestamped outline (mm:ss what you show):
0:00 - Overview
0:30 - Setup
1:30 - Running experiment
2:30 - Results
3:30 - Conclusion
### 3.4 Two-page paper deliverable (IEEE 2-column format) 
- Paper title: Paper title: Impact of WiFi Interference on Bluetooth Latency and Packet Loss
- 2 pages (PDF), includes at least **one results figure/table**

---

## 4) Results plan
### 4.1 Primary metric(s)
List 1–3 metrics you will report (examples):
- Latency (response time): how long it takes to get a reply (median and p95)
- Packet loss (%): how many messages don’t get a response
- Consistency: how much results change between different trials

### 4.2 Baselines
List at least 2 baselines (fair comparison points).

- Baseline A:Bluetooth test with no WiFi traffic running
- Baseline B:Bluetooth test with active 2.4 GHz WiFi interference
- (Optional) Baseline C:Bluetooth test with lighter WiFi traffic than Baseline B

### 4.3 Controlled variables / sweep grid
List at least 2 variables you will sweep.

| Variable | Values |
|---|---|
| WiFi condition | None, Active 2.4 GHz interference |
| Trial number | 10 trials per condition |

### 4.4 Required figures/plots/tables
List the **exact** outputs you promise to deliver.

| Item | X-axis | Y-axis | Sweep/conditions | Output filename |
|---|---|---|---|---|
| Figure 1 | Latency | CDF | No WiFi vs WiFi interference | latency_cdf.png |
| Figure 2 | Condition | Latency | No WiFi vs WiFi interference | latency_boxplot.png |
| (Optional) Table 1 | Condition | Summary stats | latency,packet loss, trials | summary_stats.csv |

---

## 5) Implementation plan
### 5.1 Architecture sketch
- What are the main components?
- How do they connect?
Main components:
- Laptop running the measurement script
- ESP32 responding to Bluetooth messages
- WiFi source creating 2.4 GHz interference
- Python analysis script for CSV and plots

How they connect:
- The laptop sends a Bluetooth ping to the ESP32
- The ESP32 immediately sends a reply back
- The laptop measures how long the round trip took
- During interference trials, WiFi traffic runs at the same time

Include a simple diagram or dependency sketch.
Laptop -> Bluetooth ping -> ESP32
Laptop <- Bluetooth reply <- ESP32

WiFi source/router -> 2.4 GHz traffic during interference trials

### 5.2 Key data structures
List the core structures you will maintain:
- what they store
- invariants (what must always be true)
- where they live (module/file)

- packet_log
Stores each packet attempt (time, condition, latency, success)
One row per packet
Saved in raw CSV from run_experiment.py

- trial_results
Stores results per trial (sent, received, loss, latency stats)
Sent ≥ received
Saved in results/trial_results.csv

- summary_stats
Stores overall results per condition
One row per condition
Saved in results/summary_stats.csv
---


## 6) Validation checks
You must include at least **two** checks that prove your measurements are meaningful.

| Validation check | What evidence you will record | Pass condition |
|---|---|---|
| Bluetooth connection stays active | connection log / no disconnect messages | no disconnect during a valid trial |
| WiFi interference is actually present | WiFi traffic log or throughput reading | WiFi traffic is active during the full interference trial |

---

## 7) Checkpoints and deadlines
Your team must meet **all** checkpoints.

### Checkpoint 0: Approval
- [ ] Contract is specific enough to approve (baselines + sweeps + outputs + commands)
- [ ] Repo created with skeleton + README stub

### Checkpoint 1: Baseline
- [ ] Baseline run works end-to-end
- [ ] At least one preliminary figure/table generated from real measurements/data
- [ ] Evidence recorded for validation checks (initial)

### Checkpoint 2: Pipelines
- [ ] Full experiment pipeline runs end-to-end
- [ ] CSV output + at least two final-quality figures (or 1 figure + 1 table)

### Checkpoint 3: Final
- [ ] Demo video complete (timestamped)
- [ ] Repo reproducible from README commands
- [ ] 2-page paper PDF submitted

---

## 8) Team plan (required)
### 8.1 Ownership
| Task | Owner | Due checkpoint | Definition of done |
|---|---|---|---|
| Literature scan + delta bullets | Odey | Checkpoint 0 |  |
| Harness / pipeline | Kevin/Andrew | Checkpoint 1 |  |
| Experiments + CSV | Kevin/Andrew | Checkpoint 2 |  |
| Plotting + figures | Odey | Checkpoint 2 |  |
| Paper draft | Odey | Checkpoint 3 |  |
| Demo video | Kevin | Checkpoint 3 |  |

### 8.2 Meetings
- How often you will sync as a team:
- How you will communicate:
- 2 to 3 times per week
- group chat, Discord, and in person
---

## 9) Edge cases and tests
### 9.1 Top edge cases you will test
| Edge case | Why it matters | How you will test | Expected behavior |
|---|---|---|---|
| No WiFi interference (baseline) | Establishes reference performance with no congestion | Run Bluetooth test with WiFi disabled | Low latency and near-zero packet loss |
| Active 2.4 GHz WiFi interference | Tests real-world congestion in shared band |Use phone hotspot on 2.4 GHz while running trials| Increased latency and higher packet loss |
| High congestion environment | Many devices in same band increase interference | Run hotspot  | Further increase in latency and packet loss |
| Weak Bluetooth signal (distance/placement) | Signal quality can affect reliability | Slightly change device placement or distance | Higher latency variability and possible packet loss |
### 9.2 Minimal test plan
- Integration tests (required): e.g., “run pipeline and verify outputs exist + schema sanity”
- Unit tests (optional):
- results/trial_results.csv, results/summary_stats.csv, and required plot files
---

## 10) Instructor/TA approval notes (To be filled out after being graded)
- Decision: (Approved / Revise and resubmit)
- Notes:
