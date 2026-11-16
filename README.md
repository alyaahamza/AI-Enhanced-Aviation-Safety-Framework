
# AI-Enhanced Framework for Resilient Navigation, Cumulonimbus Detection, and Synthetic Landing Guidance

**Authors:** Rehan Ahmed Yosef, Alyaa A. Hamza, Kamel Hussien Rahouma

**Corresponding Author:** Dr. Alyaa A. Hamza  
**Email:** Alyaa.hamza@sut.edu.eg  
**Journal:** Scientific Reports (manuscript under revision)

---

## Overview

This repository provides experimental documentation, dataset information, implementation references, and reproducibility materials associated with the manuscript **"AI-Enhanced Framework for Resilient Navigation, Cumulonimbus Detection, and Synthetic Landing Guidance."**

The proposed AI-enhanced aviation decision-support framework consists of three complementary research modules:

1. **Module 1 — Navigation-Error Compensation:** Hybrid Recurrent Neural Network and Error-State Kalman Filter (RNN–ESKF) for trajectory correction under simulated GNSS positioning degradation.
2. **Module 2 — Cumulonimbus Classification:** MobileNetV2-based transfer learning for cumulonimbus cloud detection.
3. **Module 3 — Synthetic Landing Guidance:** Generation of synthetic localizer and glide-slope deviation quantities using recorded aircraft trajectories.

**Research scope:** The three modules were evaluated independently. The study does not demonstrate an integrated operational flight-management system, autonomous storm avoidance, or certified precision landing.

---

## Repository Contents

| Component | Description |
|---|---|
| `README.md` | Research overview and experimental results |
| `DATA_AVAILABILITY.md` | Data sources and access conditions |
| `module1_navigation/` | RNN–ESKF navigation correction |
| `module2_cumulonimbus/` | MobileNetV2 cloud classification |
| `module3_landing/` | Synthetic approach guidance |
| `results/` | Experimental results and figures |

---

## Module 1: Navigation-Error Compensation Using RNN–ESKF

### Objective

Evaluate the ability of a hybrid RNN–ESKF model to correct aircraft trajectory-position errors under simulated GNSS degradation.

### Dataset

**Source:** OpenSky Network ADS-B aircraft trajectory archives

**Processed dataset:** `cleaned_states.csv`

The processed dataset includes:

- Latitude and longitude
- Velocity and heading
- Vertical rate
- Altitude
- Timestamps
- Aircraft identification (`icao24`)

**Data Availability:** The processed dataset `cleaned_states.csv` is not publicly hosted in this repository. It is available from the corresponding author on reasonable request, subject to applicable source-data access and redistribution conditions.

### Implementation

The experimental workflow includes:

1. Aircraft trajectory preprocessing.
2. Synthetic positioning-error generation.
3. Sequential RNN-based trajectory-change prediction.
4. Error-State Kalman filtering.
5. RMS positional error and CEP50 evaluation.

**Code:** [Module 1 — Google Colab](https://colab.research.google.com/drive/1-ls9Orr9nyGTcrCgSC1askNKTguz2wg7)

### Experimental Results

| Evaluation Metric | Baseline | RNN–ESKF |
|---|---:|---:|
| RMS positional error | 120.3 m | 14.2 m |
| CEP50 | 85.2 m | 8.8 m |

The reported RMS positional error reduction is approximately **88.2%** under the selected simulation conditions.

### Limitations

The experiment uses recorded ADS-B observations and synthetically degraded positions rather than synchronized raw IMU measurements. The results do not establish performance under physical INS sensor drift or operational GNSS interference.

---

## Module 2: Cumulonimbus Classification Using MobileNetV2

### Objective

Develop and evaluate a binary cloud-image classifier for distinguishing cumulonimbus (Cb) clouds from other cloud classes.

### Dataset

**Name:** Cirrus Cumulus Stratus Nimbus (CCSN)

**Source:** Harvard Dataverse

**Dataset DOI:** https://doi.org/10.7910/DVN/CADDPD

The original dataset contains 2,543 images across 11 cloud categories.

After removing 17 duplicate images, 2,526 unique images remained for the classification experiment.

### Dataset Distribution

| Subset | Non-Cb | Cb | Total |
|---|---:|---:|---:|
| Training | 1,603 | 165 | 1,768 |
| Validation | 343 | 36 | 379 |
| Testing | 344 | 35 | 379 |
| Total | 2,290 | 236 | 2,526 |

### Implementation

- MobileNetV2 transfer learning.
- Binary cross-entropy loss.
- Inverse-frequency class weighting.
- Training-only image augmentation.
- Validation-based threshold selection.
- Independent test-set evaluation.

**Code:** [Module 2 — Kaggle Notebook](https://www.kaggle.com/code/alyaashams/aviation-paper-3-module-2)

### Experimental Results

| Metric | Result |
|---|---:|
| Test images | 379 |
| True positives | 33 |
| True negatives | 294 |
| False positives | 50 |
| False negatives | 2 |
| Overall accuracy | 86.28% |
| Balanced accuracy | 89.88% |
| Cb precision | 39.76% |
| Cb recall | 94.29% |
| Cb F1-score | 55.93% |
| ROC-AUC | 0.961 |
| PR-AUC | 0.665 |
| Decision threshold | 0.312 |

### Limitations

The experiment demonstrates image classification only. The dataset does not contain geographically referenced storm locations, aircraft-to-cloud distances, storm motion, or the information necessary to validate autonomous weather-avoidance maneuvers.

The 39.76% Cb precision also indicates a substantial false-alarm proportion.

---

## Module 3: Synthetic Landing Guidance

### Objective

Investigate the generation of synthetic lateral and vertical approach-guidance quantities from recorded aircraft trajectory observations.

### Dataset

**Name:** Flight Trajectory Dataset

**Source:** Kaggle

**Dataset:** [Flight Trajectory Dataset](https://www.kaggle.com/datasets/minhnguyen181121351/flight-trajectory-dataset)

The dataset contains aircraft surveillance trajectories with geographical coordinates, altitude, velocity, heading, timestamps, and aircraft identification.

### Experimental Workflow

1. Clean and filter aircraft trajectory records.
2. Identify candidate aircraft and trajectory segments.
3. Select a terminal trajectory segment.
4. Define a synthetic runway threshold.
5. Transform geographical coordinates into local Cartesian coordinates.
6. Estimate synthetic runway alignment using principal-component analysis.
7. Calculate synthetic lateral and vertical approach deviations.

**Code:** [Module 3 — Kaggle Notebook](https://www.kaggle.com/code/alyaashams/aviation-paper-3-module-3)

### Reported Dataset Processing

| Parameter | Value |
|---|---:|
| Original observations | 21,767,257 |
| Observations after cleaning | 21,576,990 |
| Candidate aircraft | 2,158 |
| Selected ICAO24 | a42f04 |
| Selected segment | 476 |
| Segment observations | 127 |
| Final-approach samples extracted | 50 |
| Samples used for metric calculation | 32 |
| Synthetic glide-slope angle | 3° |

### Limitations

The experimental data do not include verified runway threshold coordinates, measured localizer and glide-slope signals, or synchronized LiDAR and camera observations.

Therefore, the results demonstrate synthetic guidance computation and do not establish certified precision-approach accuracy or operational landing capability.

---

## Data Availability

The datasets analysed during the current study were obtained from existing third-party sources.

- **Module 1:** Aircraft trajectory observations were derived from the OpenSky Network ADS-B archives. The processed dataset `cleaned_states.csv` is available from the corresponding author on reasonable request, subject to applicable data-use and redistribution conditions.
- **Module 2:** The CCSN dataset is publicly accessible through [Harvard Dataverse](https://doi.org/10.7910/DVN/CADDPD).
- **Module 3:** The Flight Trajectory Dataset is accessible through [Kaggle](https://www.kaggle.com/datasets/minhnguyen181121351/flight-trajectory-dataset).

For requests relating to Module 1, contact **Alyaa.hamza@sut.edu.eg**.

---

## Code Availability

The experimental implementation notebooks are linked below:

- [Module 1 — Google Colab](https://colab.research.google.com/drive/1-ls9Orr9nyGTcrCgSC1askNKTguz2wg7)
- [Module 2 — Kaggle](https://www.kaggle.com/code/alyaashams/aviation-paper-3-module-2)
- [Module 3 — Kaggle](https://www.kaggle.com/code/alyaashams/aviation-paper-3-module-3)

Notebook sharing permissions should be verified to ensure that public visitors can access the code.

---

## Reproducibility

To reproduce the experimental workflows:

1. Obtain the corresponding datasets from their original sources or through the stated request procedure.
2. Review the experimental preprocessing and filtering procedures.
3. Access the relevant implementation notebook.
4. Use the specified training, validation, and testing procedures.
5. Compare the reproduced outputs with the reported results.

Successful reproduction depends on access to the appropriate processed data, code versions, dependencies, and experimental configurations.

---

## Study Limitations

The proposed framework is an architectural proof of concept supported by separate module-level evaluations.

The study does not establish:

- End-to-end integration of the three modules.
- Operational INS drift compensation with raw inertial measurements.
- Real-time autonomous cumulonimbus avoidance.
- Certified ILS-equivalent landing guidance.
- Autonomous aircraft landing.
- Flight-tested safety or operational reliability.

Further validation is required using synchronized multisensor data, independent flight trajectories, real meteorological observations, verified runway geometry, and integrated aviation test environments.

---

## Citation

If you use this repository or its associated methodology, please cite:

Yosef, R. A., Hamza, A. A., and Rahouma, K. H. **AI-Enhanced Framework for Resilient Navigation, Cumulonimbus Detection, and Synthetic Landing Guidance.** Manuscript under revision, *Scientific Reports*.

Please also cite the original third-party datasets according to their respective citation requirements.

---

## Contact

**Dr. Alyaa A. Hamza**  
Faculty of Engineering Technology  
Elsewedy University of Technology–Polytechnic of Egypt  
Email: Alyaa.hamza@sut.edu.eg
