# Drift-Resilient Anomaly Detection Benchmark

Author: Rakan M. AlKhulaif
Paper: Benchmarking Drift-Resilient Anomaly Detection in Streaming Industrial Data: A Case Study on Turbofan Engine Failures
Status: Independent research

This repo contains code and results for benchmarking unsupervisd anomaly detection under concept drift on NASA C-MAPSS FD001. We simulate gradual drift and inject synthetic anomalies, then evaluate Isolation Forest, One-Class SVM, and ADWIN (used as a naive anomaly proxy). Static detectors degrade under drift, fixed retraining does not recover performance, and drift-change signals misalign with anomalies.

Layout:

- JURI_Submission.ipynb  end to end notebook
- results/juri_revision   artifacts for Units 1 to 3 (json, csv, png)

Data:
Use the C-MAPSS FD001 training set.
Expected paths:
CMaps/train_FD001.txt
CMaps/test_FD001.txt
CMaps/RUL_FD001.txt

If running in Colab, upload your archive and extract to /content/cmapss_data/CMaps.

Environment:

Preferred
pip install -r requirements-lock.txt

Minimal
pip install pandas==2.2.2 numpy==1.26.4 scikit-learn matplotlib
pip install river==0.21.2
pip install pyod --no-deps

To reproduce in Colab:

1) Upload archive.zip and extract:
from google.colab import files
uploaded = files.upload()
import zipfile
with zipfile.ZipFile("archive.zip", "r") as z:
    z.extractall("/content/cmapss_data")

2) Open JURI_Submission.ipynb

3) Load FD001 and slice units:
import pandas as pd
cols = ["unit","cycle"] + [f"op_{i}" for i in range(1,4)] + [f"sensor_{i}" for i in range(1,22)]
df_fd001 = pd.read_csv("/content/cmapss_data/CMaps/train_FD001.txt", sep=" ", header=None).dropna(axis=1, how="all")
df_fd001.columns = cols
df_unit1 = df_fd001[df_fd001["unit"] == 1].reset_index(drop=True)
df_unit2 = df_fd001[df_fd001["unit"] == 2].reset_index(drop=True)
df_unit3 = df_fd001[df_fd001["unit"] == 3].reset_index(drop=True)

4) Run the notebook cells for anomaly injection, drift, normalisation, detectors, metrics

5) Expected outputs
results/juri_revision/cross_unit_summary.csv
results/juri_revision/unit_1_metrics.json
results/juri_revision/unit_2_metrics.json
results/juri_revision/unit_3_metrics.json
results/juri_revision/fig5_unit_1_adwin_vs_anomalies.png
results/juri_revision/fig5_unit_2_adwin_vs_anomalies.png
results/juri_revision/fig5_unit_3_adwin_vs_anomalies.png

Reproduce it locally:

python -m venv .venv
source .venv/bin/activate
pip install -r requirements-lock.txt
jupyter notebook
Open JURI_Submission.ipynb, load FD001 as above, run for units 1 to 3

Citation:

@misc{alkhulaif2025drift,
  title  = {Benchmarking Drift-Resilient Anomaly Detection in Streaming Industrial Data: A Case Study on Turbofan Engine Failures},
  author = {AlKhulaif, Rakan},
  year   = {2025},
  note   = {Independent undergraduate research},
  url    = {https://github.com/Acquiesense/cmapss-drift-benchmark}
}
