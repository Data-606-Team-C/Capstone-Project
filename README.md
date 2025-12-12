# Predicting Hospital Readmission

Data606 Team C:

Brett Duvall

Mohammedamin Mussa

Luis Vargas Ramirez


DATASET
                                          
Size: 101,766 encounters, 50 variables, 18 MB.
Categories: demographics, diagnoses, labs (A1C, glucose), medication use, utilization.
Target: 30 day readmission.
https://archive.ics.uci.edu/dataset/296/diabetes%2B130-us%2Bhospitals%2Bfor%2Byears%2B1999-2008



READMISSION PREDICTION

This project's objective is to develop a machine learning model to predict patients likely to be readmitted within 30 days (<30), enabling the hospital to intervene early and avoid CMS penalties.

1. Data Strategy & Integrity

Leakage Prevention: Implemented GroupShuffleSplit on patient_nbr. This ensures a patient's future visits never leak into the training set, preventing the "time-travel" error common in medical datasets.
Cohort Selection: Removed discharge records related to "Hospice" or "Expired" (Death), as readmission prediction is irrelevant for these cases.
Handling Missing Data: Treated "missing" as a deliberate category (e.g., for payer_code or medical_specialty) rather than imputing, preserving valuable signal about administrative gaps.

2. Target Definition Strategy We defined the prediction target using a binary approach:

Binary: <30 (Urgent) vs. All Others (Stable).
Result: Selected Strategy. This isolated the minority class (11% prevalence) and focused the model on the specific problem of immediate readmission penalties.

3. Feature Engineering Breakthrough

"Visit Count" Feature: We engineered a historical feature tracking a patient’s cumulative visits (visit_count).
Impact: Feature Importance analysis confirmed this is consistently a Top-3 predictor, proving that a patient's history of instability is just as important as their current lab results.

4. Modeling Results We progressed through three levels of model complexity to find the "ceiling" of the data:

Baseline (Logistic Regression): Plateaued at 0.66 AUC. Established that linear relationships alone were insufficient.
Intermediate (Random Forest): Stuck at 0.66 AUC. Failed to outperform the linear model, suggesting the signal is highly complex.
Champion (XGBoost): Broke the ceiling with 0.672 AUC (before tuning). Gradient boosting successfully captured complex, non-linear interactions.

5. Final Optimization (Completed)

Action: Performed 5-Fold Grid Search Cross-Validation to tune XGBoost hyperparameters.
Result: Achieved a Final AUC of 0.675.



REFERENCES

Ashfaq, A., Sant’Anna, A., Lingman, M., & Nowaczyk, S. (2019). Readmission prediction using deep learning on electronic health records. Journal of Biomedical Informatics, 97, 103271. https://doi.org/10.1016/j.jbi.2019.103271

Emi-Johnson O., Nkrumah K. "Predicting 30-Day Hospital Readmission in Patients With Diabetes Using Machine Learning on Electronic Health Record Data." (April 17, 2025), Cureus 17(4): e82437. DOI 10.7759/cureus.82437

Shukla, S., & Tripathi, S. P. (2020). EmbPred30: Assessing 30-days readmission for diabetic patients using categorical embeddings. arXiv preprint, arXiv:2002.11215. https://arxiv.org/abs/2002.11215

Strack, B., DeShazo, J. P., Gennings, C., Olmo, J. L., Ventura, S., Cios, K. J., & Clore, J. N. (2014). Impact of HbA1c measurement on hospital readmission rates: Analysis of 70,000 clinical database patient records. BioMed Research International, 2014, 781670. https://doi.org/10.1155/2014/781670

Wang, S., & Zhu, X. (2021). Predictive modeling of hospital readmission: Challenges and solutions. arXiv preprint, arXiv:2106.08488. https://arxiv.org/abs/2106.08488

Burrill, L. UC Berkeley (2025). diabetes_readmission [Computer software]. https://github.com/lelandburrill/diabetes_readmission



