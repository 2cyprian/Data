# Board Report: Customer Churn AI – Status and Decisions

Date: 2026-03-15
Audience: Board and Leadership

## Executive Summary
A churn prediction model is now running after resolving a data-format issue. Current accuracy is 78.64%, but the model detects only about half of churners (recall 49%). This is not yet sufficient for strong retention impact. The recommendation is to move to a targeted improvement phase before broad rollout.

## What was wrong
The first model run failed because text values were sent to an algorithm that accepts only numeric inputs.

## What was fixed
The team corrected preprocessing by:
- removing identifier fields,
- converting text-based numeric values,
- encoding categorical fields,
- and retraining successfully.

## Current performance in business terms
- Good at identifying customers who stay.
- Moderate at identifying customers likely to leave.
- Main risk: missed churners reduce retention campaign effectiveness.

## Decision requested from Board
Approve a 4–6 week optimization sprint focused on churner detection quality before production-scale deployment.

## Proposed plan
1. Improve model to raise churn recall.
2. Tune decision threshold based on retention economics (cost to contact vs value saved).
3. Validate with cross-validation and holdout tests.
4. Deploy in controlled pilot (limited customer segment).
5. Scale only if KPI targets are met.

## KPI gates for scale-up
- Churn recall >= 70%
- Positive retention ROI in pilot
- Stable model performance over 2 consecutive evaluation periods

## Governance and risk controls
- Monthly model monitoring and retraining policy
- Drift and quality alerts
- Clear ownership: Data team (model), CRM team (campaign operations), Finance (ROI tracking)

## What to avoid going forward
- Training without strict preprocessing checks
- Using default probability threshold without business-cost analysis
- Deploying without pilot validation and monitoring controls

## Ask
Approve resources for optimization sprint and pilot execution, with go/no-go review at sprint completion.
