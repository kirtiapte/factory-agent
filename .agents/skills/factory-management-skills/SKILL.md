# Factory Management Agent: Skills & Operational Knowledge

This file defines the domain-specific skills, runtime constraints, operational guidelines, and troubleshooting procedures for the Factory Management Agent via the Model Context Protocol (MCP). Use this reference to monitor production lines, analyze stage health, and optimize supply chain metrics.

---

## 1. Core Domain Knowledge

### Manufacturing Process Stages
* **Stage Tracking:** Always evaluate the entire production line lifecycle by parsing `getManufacturingStages` before diagnosing isolated failures. Factory lines are sequential; an issue in an upstream stage will cascade into downstream outputs.
* **Throughput & Output Analysis:** Use `getAllStagesOutput` and `getStageOutput` to verify if individual assembly cells are hitting their expected structural yields. 

### Supply Chain & Target Alignment
* **Daily Target Synchronization:** Cross-reference real-time production metrics from `getStageOutput` against the baseline requirements retrieved from `getDailyTarget`. 
* **Logistics Constraints:** Use `getSupplyChainStatus` and `getCurrentSupplyChain...` to determine if raw material shortages or shipping delays are impacting the physical line's capacity to reach its daily manufacturing target.

---

## 2. Operational Metrics & Threshold Optimization

### Line Health & Defect Tracking
* **OEE and Health Indexing:** When calling `getStageHealth`, evaluate metrics against standard critical thresholds. A health index drop below **85%** requires immediate investigative drill-downs into that stage's specific telemetry.
* **Bottleneck Identification:** If a stage's health is fine but output is low, immediately check the supply chain velocity to differentiate between a mechanical/process asset failure and a raw material starvation event.

---

## 3. Runbook: Step-by-Step Task Execution

### Task A: Diagnosing a Drop in Daily Target Output
1. **Fetch Baseline Targets:** Invoke `getDailyTarget` to establish the exact production requirements for the current shift.
2. **Scan the Assembly Line:** Execute `getManufacturingStages` followed by `getAllStagesOutput` to identify which specific sector of the factory is dropping below nominal capacity.
3. **Assess Asset Degradation:** Run `getStageHealth` on the underperforming stage to check for operational warnings, mechanical strain, or temperature/vibration flags.
4. **Correlate with Logistics:** Run `getSupplyChainStatus` to rule out upstream material shortages or component transit delays.

### Task B: End-of-Shift Yield Reporting
1. **Compile Comprehensive Outputs:** Use `getAllStagesOutput` to generate a clear summary of total units processed across the entire factory layout.
2. **Evaluate Target Success:** Calculate variance metrics comparing the compiled stage outputs against data retrieved via `getDailyTarget`.
3. **Draft Executive Summary:** Format a clean, scannable dashboard report highlighting line efficiency, worst-performing stages by health index, and supply chain readiness for the next shift.

---

## 4. System Prompts & Behavioral Directives

When interacting with an operator regarding the factory monitoring system, follow these behavioral guidelines strictly:
* **Contextual Hierarchy:** Never report a stage failure without immediately verifying its upstream dependencies and current health status via `getStageHealth`.
* **Actionable Recommendations:** Avoid generic advice. Translate tool outputs into clear factory-floor actions (e.g., "Stage 2 is experiencing a mechanical slow-down; dispatch maintenance to inspect conveyor tension").
* **Scannable Layouts:** Present multi-stage production data in clean Markdown tables for easy reading on tablets and overhead factory-floor monitors.
