# Predictive Maintenance System — Machine Failure Prediction

A machine learning project that predicts whether a machine is about to fail, using sensor
readings like temperature, rotational speed, torque, and tool wear. Built on the AI4I 2020
Predictive Maintenance Dataset (10,000 machine records).

## Problem

Machines break down unexpectedly, which is costly and, in some settings, dangerous. Instead
of waiting for a failure or servicing equipment on a fixed schedule regardless of actual
condition, this project uses sensor data to predict which machines are at risk of failing
soon — so maintenance can happen before a breakdown, not after.

## What I did

1. **Cleaned the data** — removed ID columns and columns that would leak the answer
   (like failure-type labels that are only known after a failure already happened).
2. **Handled class imbalance** — only about 3.4% of machines in the dataset actually
   failed, so a model can get high accuracy just by predicting "no failure" every time.
   I used SMOTE (creates synthetic examples of the rare failure cases) and XGBoost's
   `scale_pos_weight` setting (tells the model to pay more attention to the rare class)
   to fix this.
3. **Compared two models** to see the trade-off between catching real failures and
   avoiding false alarms.
4. **Built a simple interactive tool** where you can enter live sensor values and get
   a failure risk prediction, with a choice between a conservative or high-safety mode.

## Results

| Model | Accuracy | Precision | Recall |
|---|---|---|---|
| Random Forest (baseline) | 98.25% | 88.37% | 55.88% |
| XGBoost (tuned) | 92.95% | 31.47% | 91.18% |

**Accuracy** is how often the model is right overall.
**Precision** is: when the model says "failure," how often is it actually right.
**Recall** is: out of all the real failures, how many did the model actually catch.

The Random Forest model looks great on accuracy, but it misses 44% of real failures —
it plays it safe by rarely predicting failure, so it looks accurate mostly because
failures are rare to begin with.

The XGBoost model gives more false alarms, but catches 91% of real failures.

## Why I picked recall over accuracy

In this problem, missing a real failure is much worse than a false alarm. A false alarm
just means an unnecessary inspection. A missed failure means a machine breaks down with
no warning — costly, and potentially unsafe if it's critical equipment. So I treated the
XGBoost model as the better choice for anything mission-critical, even though its
accuracy number looks lower on paper.

## What I learned

Accuracy alone can be misleading on imbalanced data. A model can look "good" while
still missing most of what actually matters. Precision and recall tell a more honest
story, and the right trade-off between them depends on the real cost of being wrong —
not just which model has the highest score.

## What I'd improve with more time

- Use cross-validation instead of a single train/test split, for more reliable results
- Try feature engineering, like tracking how sensor readings change over time
- Test the model on a completely separate dataset to check it generalizes
- Assign real costs to false alarms vs missed failures, and optimize for that directly

## Tech used

Python, pandas, scikit-learn, imbalanced-learn (SMOTE), XGBoost, matplotlib
