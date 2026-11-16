# AI-Enhanced-Aviation-Safety-Framework


Overview

This repository documents the experimental datasets, implementation notebooks, and reproducibility materials associated with a modular AI-enhanced aviation decision-support framework.

The framework addresses three complementary research areas:

Navigation-error compensation under simulated GNSS positioning degradation using a hybrid Recurrent Neural Network and Error-State Kalman Filter (RNN–ESKF).

Cumulonimbus cloud-image classification using MobileNetV2 transfer learning as a preliminary component of convective-weather hazard screening.

Synthetic lateral and vertical approach-guidance generation using recorded aircraft trajectories and inferred runway-reference geometry.

The study evaluates these modules independently. It does not establish an end-to-end integrated airborne system, autonomous storm avoidance, or certified precision-landing capability.

Module 1: Navigation-Error Compensation Using RNN–ESKF

Objective

To investigate whether data-driven sequential modeling combined with Kalman filtering can reduce position-estimation errors during simulated GNSS degradation.

Dataset

The experiment uses aircraft trajectory observations derived from the OpenSky Network's ADS-B archives and processed into a study-specific dataset named cleaned_states.csv.

The processed data include geographical position, velocity, heading, vertical rate, altitude, timestamps, and aircraft identification fields.

Because synchronized raw IMU measurements are not included, the experiment investigates correction of synthetically degraded aircraft trajectories rather than direct compensation for physical inertial sensor drift.

Implementation

The experimental workflow combines:

Sequential aircraft-motion feature preparation.

Simulation of positioning degradation.

RNN-based prediction of trajectory changes.

Error-State Kalman filtering.

Evaluation using RMS positional error and CEP50.

Implementation notebook: Module 1 — Google Colab

Reported Results

Metric

Baseline

RNN–ESKF

RMS positional error

120.3 m

14.2 m

CEP50

85.2 m

8.8 m

The reported RMS reduction is approximately 88.2% under the selected simulation conditions. These findings are not evidence of operational navigation certification or generalization to physical IMU drift.

Module 2: Cumulonimbus Classification Using MobileNetV2

Objective

To evaluate a transfer-learning image classifier for distinguishing cumulonimbus (Cb) clouds from other cloud types.

Dataset

The experiment uses the Cirrus Cumulus Stratus Nimbus (CCSN) dataset, containing images representing 11 cloud categories.

Dataset: CCSN — Harvard Dataverse

The original dataset contained 2,543 images. Following removal of 17 duplicates, 2,526 images remained for the binary classification experiment.

Experimental Design

Binary classes: Cb and non-Cb.

Training, validation, and independent test subsets.

Inverse-frequency class-weighted training.

MobileNetV2 transfer learning and fine-tuning.

Validation-based threshold selection.

Evaluation emphasizing minority-class recall, precision, and false alarms.

Implementation notebook: Module 2 — Kaggle

Reported Results

Metric

Result

Test images

379

True positives

33

True negatives

294

False positives

50

False negatives

2

Accuracy

86.28%

Balanced accuracy

89.88%

Cb precision

39.76%

Cb recall

94.29%

Cb F1-score

55.93%

ROC-AUC

0.961

PR-AUC

0.665

Selected threshold

0.312

The classifier prioritizes Cb detection sensitivity, but its relatively low precision produces substantial false alarms. The study evaluates image classification only, not airborne storm localization or autonomous avoidance.

Module 3: Synthetic Landing-Guidance Generation

Objective

To investigate the mathematical generation of synthetic lateral and vertical approach-guidance quantities from recorded aircraft trajectory data.

Dataset

The experiment uses the publicly hosted Flight Trajectory Dataset available on Kaggle.

Dataset: Flight Trajectory Dataset — Kaggle

The source dataset contains aircraft surveillance observations including geographical coordinates, altitude, velocity, heading, timestamps, and aircraft identifiers.

Experimental Design

The reported workflow consists of:

Trajectory cleaning and selection.

Identification of candidate aircraft trajectories.

Selection of a trajectory segment for guidance analysis.

Construction of a synthetic runway threshold and reference axis.

Local East–North coordinate transformation.

Principal-component analysis of terminal trajectory geometry.

Calculation of synthetic lateral and vertical deviation quantities.

Implementation notebook: Module 3 — Kaggle

Reported Dataset Processing

Parameter

Value

Original observations

21,767,257

Observations after cleaning

21,576,990

Candidate aircraft

2,158

Selected ICAO24

a42f04

Selected segment

476

Segment observations

127

Extracted approach samples

50

Samples used for metric calculation

32

Synthetic reference glide-slope angle

3°

These are the values reported in the manuscript and are subject to verification of the selected trajectory's anomalous altitude profile.

The dataset does not provide verified runway threshold coordinates, actual ILS measurements, or synchronized LiDAR and camera observations. Therefore, the experiment demonstrates synthetic guidance computation rather than validated aircraft landing performance.

Reproducibility

The original experimental workflows are associated with the linked Google Colab and Kaggle notebooks.

Researchers wishing to reproduce the experiments should:

Obtain the corresponding source datasets under the providers' access conditions.

Review the preprocessing and filtering procedures described in the manuscript.

Use the original experimental notebooks and dependencies for each module.

Reproduce training, validation, and evaluation procedures without using test-set observations for model selection.

Compare the resulting outputs with the reported tables and figures.

Availability of the original datasets does not necessarily imply that the study-specific processed files or trained model weights are included in this repository.

Data Availability

The study uses third-party aircraft surveillance and cloud-image datasets. The CCSN dataset is available through Harvard Dataverse, and the Flight Trajectory Dataset is available through Kaggle. The navigation-correction experiment uses OpenSky-derived ADS-B trajectory observations processed into cleaned_states.csv.

Dataset provenance, access links, and any restrictions on redistribution should be documented in DATA_AVAILABILITY.md.

Study Limitations

The present repository supports a proof-of-concept research study.

The reported results do not demonstrate:

End-to-end integration of all three modules.

Operational navigation reliability under actual GNSS interference.

Geographically referenced cumulonimbus avoidance.

Certified ILS-equivalent landing guidance.

Autonomous aircraft landing or flight-control capability.

Additional validation with synchronized aviation sensors, independent flight data, operational meteorological observations, and verified runway references is needed.

Citation

If you use this repository or the associated methodology, please cite the corresponding manuscript once its bibliographic details become available.


Contact

Corresponding author: Alyaa A. Hamza

Faculty of Engineering Technology, Elsewedy University of Technology–Polytechnic of Egypt

Email: Alyaa.hamza@sut.edu.eg
