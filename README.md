# 🛰️ 505 ABW: Predictive Maintenance & Failure Prediction Core

An advanced, production-ready Machine Learning system engineered to ingest real-time machinery telemetry logs and forecast equipment breakdowns before they occur. Tailored specifically for military workshop environments where operational safety overrides raw processing efficiency.

## 🛠️ Engineering Architecture
1. **Target Leakage Elimination:** Purged retrospectively logged failure type labels (`TWF`, `HDF`, etc.) to force the model to identify true predictive physical gauge patterns.
2. **Mitigating Extreme Data Skew:** Addressed severe class imbalance (~3.4% failure probability) utilizing **XGBoost Scale-Position Weighting** to prevent undetected machinery drops.
3. **Column Re-Parsing:** Re-mapped bracketed sensor metrics (`[K]`, `[rpm]`, `[Nm]`) to plain text strings to prevent core deployment system token crashes.

## 📊 Dual-Engine Performance Strategy (The Precision-Recall Trade-off)
Rather than deploying a single generic algorithm, this system integrates two distinct models to let operators match the automation sensitivity to the asset's mission-critical priority:

| Diagnostic Architecture | Operational Accuracy | Precision (False Alarm Rate) | Recall (Fault Catch Rate) | Practical Workshop Deployment |
| :--- | :---: | :---: | :---: | :--- |
| **Engine A: Conservative (Random Forest)** | **98.25%** | **88.37%** (Extremely Low False Alarms) | 55.88% (Misses 44% of real failures) | Deployed on non-critical shop equipment (lathes, sanders) where continuous inspections disrupt workflow efficiency. |
| **Engine B: High-Safety (Tuned XGBoost)** | 92.95% | 31.47% (Frequent False Alarms) | **91.18%** (Intercepts 62 out of 68 failures) | **Mission-Critical Assets** (combat engines, heavy generators) where a missed breakdown risks asset safety or lives. |

## 💡 Industrial Insights
Feature importance mapping shows that **Torque (Nm)** and **Rotational Speed (RPM)** serve as the primary statistical leading indicators driving physical machinery breakdowns.

