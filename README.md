Drift-Resilient Anomaly Detection Benchmark

**Author:** Rakan M. AlKhulaif  
**Paper:** [Benchmarking Drift-Resilient Anomaly Detection in Streaming Industrial Data: A Case Study on Turbofan Engine Failures]  
**Status:** Independent Research  

This repository contains the full codebase, simulation protocol, and experimental results for our study on how unsupervised anomaly detection models degrade under concept drift. Using NASA’s C-MAPSS FD001 dataset, we simulate gradual drift and inject synthetic anomalies to benchmark:

- **Isolation Forest** (IF)
- **One-Class SVM** (OCSVM)
- **ADWIN** drift detector (used naïvely as an anomaly proxy)

Our findings show that static detectors fail under moderate drift, retraining does not restore performance, and drift signals alone are insufficient to flag anomalies.


@misc{alkhulaif2025drift,
  title={Benchmarking Drift-Resilient Anomaly Detection in Streaming Industrial Data: A Case Study on Turbofan Engine Failures},
  author={AlKhulaif, Rakan},
  year={2025},
  note={Independent undergraduate research},
  url={https://github.com/Unimportant-Cargo/drift-anomaly-benchmark}
}
To reproduce the multi-unit results used in the JURI revision:
from pathlib import Path

python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt  # or requirements-lock.txt
git checkout feat/multi-unit-juri-revision
# CLI variant:
python main.py --unit 2 --save_dir results/juri_revision/unit_2
python main.py --unit 3 --save_dir results/juri_revision/unit_3
# Or run the Colab multi-unit helper:
python scripts/run_units.py
# Optional figure:
# python scripts/plot_adwin_timeline.py
