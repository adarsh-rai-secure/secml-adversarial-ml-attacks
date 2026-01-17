# Adversarial ML Attacks: Poisoning, Model Extraction, and Evasion

## Project Summary
This project implements and evaluates several adversarial machine learning attacks against a simple supervised classification pipeline. Using a small, interpretable feature space, the goal is to observe how different attack strategies change model behavior, accuracy, and decision boundaries rather than to optimize performance.

The notebook covers availability poisoning, targeted poisoning near decision boundaries, and functional model extraction via a shadow model.

## Experimental Setup
All experiments are run on a lightweight classification task with numeric features such as sepal and petal measurements (Iris-style data). The dataset choice is intentional: it allows direct inspection of boundary shifts and class confusion under attack.

A baseline classifier trained on clean data achieves **93.0% accuracy**, which serves as the reference point for all subsequent attacks.

## Availability Poisoning (Chaff Injection)
In the availability poisoning scenario, additional low-signal samples are injected into the training set to increase variance without introducing obvious label errors.

After retraining on the poisoned dataset:
- model accuracy drops from **93.0% to 64.5%**,
- misclassifications increase primarily between adjacent classes,
- samples near feature thresholds (for example, borderline petal length values) are reassigned to different classes.

The degradation is distributed rather than traceable to any single poisoned record, which makes detection difficult using per-sample inspection.

## Targeted Poisoning (Boundary Manipulation)
Targeted poisoning is constrained to samples near the existing decision boundary. Rather than collapsing overall performance, the attack nudges the boundary so that specific regions of the feature space are misclassified.

In practice, this produces:
- selective misclassification of boundary-adjacent samples,
- relatively small changes in aggregate accuracy,
- errors that resemble natural uncertainty rather than systemic failure.

This demonstrates how global metrics can mask integrity violations affecting specific inputs of interest.

## Model Extraction (Shadow Model)
The model extraction attack trains a shadow model using only input–output queries to the target classifier. No access to training data or internal parameters is required.

In the notebook:
- the shadow model’s predictions match the target model on **96.5% of sampled inputs**,
- decision regions align closely despite architectural differences,
- the extracted model can be used as a high-fidelity surrogate for further analysis or attack development.

From a security perspective, this shows that protecting parameters alone is insufficient if prediction behavior is exposed at scale.

## Defensive Observations
Across the experiments, several patterns emerge:
- training data integrity failures can dramatically degrade performance without obvious red flags,
- boundary-local attacks are harder to detect than global degradation,
- high agreement between shadow and target models indicates meaningful intellectual property leakage.

Potential mitigations discussed in the notebook include data validation, robust training, monitoring for distribution shift, and limiting high-resolution query access to deployed models.

## How to Run
Open the notebook and execute cells in order.

The experiments use the built-in Iris dataset from scikit-learn or an equivalent CSV with the same schema. Dataset assumptions and preprocessing steps are documented inline.

## Notes on Use
This repository is intended for defensive research and understanding ML failure modes. The implementations are simplified to make attack mechanics and model behavior easy to inspect.
