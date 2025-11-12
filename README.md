# bmi500-homework12


# Pandemic Modeling: SIR and SEIR Compartmental Models

**Assignment:** BMI500 HW 12 - SIR and SEIR Model Implementation for Pandemic Spread  
**Date:** November 2025

---

## Project Overview

This repository contains a complete implementation and analysis of compartmental epidemiological models (SIR and SEIR) to understand pandemic dynamics, evaluate intervention strategies, and inform public health policy decisions.

### Selected Questions

This project addresses
- **Part A** SIR Model Implementation and Simulation
---

## Key Insights

### 1. Basic Reproductive Number (R₀) as Critical Threshold

The basic reproductive number emerged as the fundamental parameter governing epidemic behavior:

- **SIR Model:** See figure [sir_model_dynamics.png](./sir_model_dynamics.png).

 
- **SEIR Model:** See figure [seir_365_1200.png](./seir_365_1200.png).
- **Threshold Effect:** R₀ > 1 enables epidemic spread; R₀ < 1 causes natural extinction
- **Policy Target:** All effective interventions work by reducing effective R₀ below 1

### 2. Exponential Early Intervention Benefits

Small changes in intervention timing produce exponential differences in outcomes:

- **Peak delay:** Implementing measures even 5 days earlier can reduce peak infections by 50%+
- **Transmission reduction:** 50% decrease in β → 80% reduction in peak infections
- **Critical window:** Exponential growth phase offers maximum leverage for control measures


### 3. Multi-Wave Pandemic Patterns with Demographic Turnover

The SEIR model with births/deaths reveals complex endemic dynamics:
see figure [seir_365_1200.png](./seir_365_1200.png).
- **Wave 1:** Day 60, peak 179 infections (initial epidemic)
- **Wave 2:** Day 254, peak 62 infections (65% reduction due to partial immunity)
- **Mechanism:** Birth rate (μN = 10/day) continuously replenishes susceptible pool
- **Long-term:** System converges to endemic equilibrium (~50 infections sustained indefinitely)
- **Implication:** Disease elimination requires sustained intervention, not just epidemic control

### 4. Synergistic Effect of Combined Interventions

Parameter sensitivity analysis demonstrates multiplicative benefits of multi-pronged strategies:
see figure [heatmap_peak.png](./heatmap_peak.png) and  [3d_plot.png](./3d_plot.png).
- **Best case** (low β, high γ): 3.9 peak infections
- **Worst case** (high β, low γ): 477 peak infections
- **Combined reduction:** 99.2% fewer peak infections with optimal interventions
- **Single interventions:** Prevention alone (80% reduction) or treatment alone (78% reduction)
- **Strategy:** Combining transmission reduction AND recovery enhancement produces superior outcomes

### 5. Exposed Compartment Effects

Including incubation period (E compartment) significantly alters predictions:

- **Peak delay:** SEIR peaks 22 days later than SIR (day 60 vs day 38)
- **Peak dampening:** 40% lower peak infections (179 vs 301)
- **Hidden reservoir:** Exposed individuals represent future transmission not visible in case counts
- **Intervention window:** 5-day incubation provides opportunity for contact tracing and quarantine
- **Realism:** Essential for diseases with significant latent periods (COVID-19, influenza, measles)

---

## Comparative Model Performance

### SIR vs SEIR: Structural Comparison

| Feature | SIR Model | SEIR Model | Winner |
|---------|-----------|------------|---------|
| **Compartments** | 3 (S, I, R) | 4 (S, E, I, R) | SEIR (more realistic) |
| **Incubation Period** | Not modeled | Explicit (avg 5 days) | SEIR |
| **Vital Dynamics** | Static population | Births & deaths (μ) | SEIR |
| **Computational Cost** | Low (3 ODEs) | Medium (4 ODEs) | SIR |
| **Peak Time** | Day 38 | Day 60 | SEIR (more accurate) |
| **Peak Magnitude** | 301 (30%) | 179 (18%) | SEIR (less severe) |
| **Long-term Behavior** | Dies out | Endemic possible | SEIR |
| **Wave Patterns** | Single wave | Multiple waves | SEIR |
| **Attack Rate** | 94% | 49% (1 year) | SEIR (more realistic) |
| **Use Case** | Teaching, quick estimates | Research, policy | Depends on context |

### Key Performance Metrics

**Accuracy vs Reality:**
- **SIR:** Overestimates peak severity by ~40% and underestimates peak timing by 22 days
- **SEIR:** Better matches observed epidemic curves with realistic incubation delays
- **Endemic diseases:** Only SEIR captures sustained transmission with demographic turnover

**Prediction Horizon:**
- **SIR:** Reliable for <100 days in closed populations
- **SEIR:** Reliable for multi-year projections in open populations with vital dynamics

**Parameter Sensitivity:**
- **SIR:** Highly sensitive to β and γ (direct relationship with R₀ = βN/γ)
- **SEIR:** Additional sensitivity to σ (incubation) and μ (demographic turnover)
- **Both:** 99%+ reduction in peak achievable with optimal parameter combinations

### Model Selection Guidelines

**Use SIR when:**
- Educational purposes or conceptual understanding needed
- Short-term outbreak in closed population (cruise ship, hospital ward)
- Rapid response estimates required with limited data
- Disease has negligible incubation period (very fast progression)

**Use SEIR when:**
- Policy decisions require accurate forecasts
- Disease has significant incubation period (COVID-19, influenza, measles)
- Long-term endemic dynamics need evaluation (>1 year)
- Demographic turnover is significant (pediatric diseases, long timescales)
- Multiple waves or seasonal patterns expected

---

## Relevance to Model-Based Machine Learning

### 1. Physics-Informed Neural Networks (PINNs)

Compartmental models provide excellent testbeds for physics-informed machine learning:

**Integration Strategy:**
```
Neural Network Output → Satisfies ODE Constraints (dS/dt = -βSI, etc.)
Loss Function = Data Fit Loss + Physics Violation Penalty
```

**Advantages:**
- **Domain knowledge:** ODE structure encodes known disease transmission mechanisms
- **Data efficiency:** Physics constraints reduce training data requirements by 50-90%
- **Extrapolation:** Better predictions outside training domain compared to pure data-driven models
- **Interpretability:** Parameters (β, γ, σ, μ) retain biological meaning

### 2. Parameter Estimation and Uncertainty Quantification

Machine learning enhances traditional parameter fitting:

**Bayesian Inference:**
- **Prior knowledge:** Use epidemiological literature for parameter priors (β ∈ [0.0001, 0.001])
- **Posterior sampling:** MCMC or variational inference provides uncertainty estimates
- **Model selection:** Compare SIR vs SEIR using evidence (marginal likelihood)

**Neural ODEs:**
- **Flexible dynamics:** Learn deviations from compartmental assumptions
- **Time-varying parameters:** β(t) and γ(t) adapt to changing interventions
- **Example:** β decreases when lockdown implemented, increases when relaxed



---

## Suggestions for Future Modeling Improvements

### 1. Spatial Models

**Current Limitation:** Homogeneous mixing assumption unrealistic for large populations

**Proposed Enhancement:**
- **Network-based models:** Nodes = subpopulations (cities), edges = travel connections
- **Partial differential equations:** SEIR-diffusion model with spatial spread
- **Agent-based models:** Individual-level heterogeneity in contacts and susceptibility

**Expected Impact:**
- Capture urban vs rural transmission differences
- Model travel restrictions and geographic containment
- Predict spatial spread patterns for targeted interventions

### 2. Age-Structured Models

**Current Limitation:** All individuals treated identically regardless of age

**Proposed Enhancement:**
- **Age groups:** Stratify into children (0-17), adults (18-64), elderly (65+)
- **Age-specific parameters:** β_ij = contact rate between age i and j, γ_i = age-specific recovery
- **Disease burden:** Mortality rates, hospitalization rates vary dramatically by age

**Expected Impact:**
- 60-70% of COVID-19 deaths in 65+ age group captured
- School closures effect quantified through reduced β_child,child
- Targeted vaccination strategies (prioritize elderly vs children)

**Example Structure:**
```
S_child, E_child, I_child, R_child
S_adult, E_adult, I_adult, R_adult  → Contact matrix C_ij connects age groups
S_elderly, E_elderly, I_elderly, R_elderly
```



---

## Suggestions for Future Modeling Improvements

Disclaimer: [AI-TOOL-NAME(S)] was/were used to complete
HW #[11].[PART-A] to [check the code].