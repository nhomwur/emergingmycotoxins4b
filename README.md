# Emerging mycotoxin early-warning models

Code accompanying **Chapter 4b**: *Improving Machine Learning Models for Early Warning of Emerging Mycotoxins Using Regional-Scale Meteorological Data*.

No data are shared in this repository. The code is provided so the method can be inspected.

## How the notebooks relate to the chapter

Run in this order. Each notebook reads the file written by the one before.

| Step | Notebook | Used for (chapter section) | Main output |
|---|---|---|---|
| 1 | `Merging_datasets.ipynb` | *Data set*: combines the EFSA and private datasets, harmonises countries, years and crops, and links regional weather. | `merged_mycotoxins.csv` |
| 2 | `LOQ_analysis.ipynb` | *Binary target*: shows how much LOQs differ and what each normalisation rule would lose. This is the basis for using the median LOQ per mycotoxin. | LOQ frequency tables |
| 3 | `Framing_merged_datasets_to_sample_level.ipynb` | *Binary target* and descriptives: builds the target `detectedC` and collapses records to 1,736 unique samples. | `sample_level_dataset.csv` and the descriptive tables |
| 4 | `ML_comparison_by_crop_with_detectedC.ipynb` | *Models*: first screening of seven algorithms per crop, which determined the shortlist tuned in step 5. | ROC and bar plots |
| 5 | `bestML_optimization_detectedC.ipynb` | *Feature reduction*, *Models*, *Results*: SHAP feature sets, tuning with leave-one-year-out CV, validation on 2023, confusion matrices and SHAP summaries. | Metrics tables and `shap_outputs/` |

## Which output feeds which table or figure

| Chapter item | Notebook | File |
|---|---|---|
| Tables 1, 2 (sample counts) | Step 3 | `unique_samples_per_country_year.csv`, `unique_samples_per_crop_year.csv` |
| Table 5 (detection rate per crop and year) | Step 3 | `detection_rate_per_crop_year.csv` |
| Table A-3 (normalised LOQ, threshold loss) | Step 3 | `detectedC_summary_per_contaminant.csv` |
| Table A-4 (detection per crop and mycotoxin) | Step 3 | `metabolite_prevalence_per_crop.csv` |
| Table 6 (best models) | Step 5 | `all_models_validation_metrics_2023_DetectedC.csv` |
| Figures 1, 3, 5 (confusion matrices) | Step 5 | `shap_outputs/*_confusion_matrix_*.png` |
| Figures 2, 4, 6 (SHAP summaries) | Step 5 | `shap_outputs/*_shap_summary_*.png` |

## Notes

- **Target:** `detectedC`. A mycotoxin record is positive if its value exceeds the median LOQ of that mycotoxin, and a sample is positive if any record is positive. The other flags (`detected`, `detected2`, `detected3`) were only used for comparison.
- **Split:** harvest year 2023 is held out for validation; 2019–2022 is split 80/20 into train and test.
- **Model choice:** the models in Table 6 were chosen on overall performance, with MCC weighing heavily. They are not simply the highest AUC, because several combinations have a high AUC but near-zero specificity. All combinations are in `all_models_validation_metrics_2023_DetectedC.csv`.
- **Maize:** the 2023 validation set has only 19 samples.
- **Paths:** the notebooks read and write in the working directory. Input file names are set at the top of each notebook.

## Requirements

Python with `pandas`, `numpy`, `scipy`, `scikit-learn`, `xgboost`, `lightgbm`, `catboost`, `shap`, `matplotlib`. Random seed: 42.

## Contact

nina.hommels@wur.nl
