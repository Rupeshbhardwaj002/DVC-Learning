# 🚀 DVC Learning: Enterprise-Grade Data Versioning

[![DVC](https://img.shields.io/badge/Data-Versioning-blue?logo=dvc&style=for-the-badge)](https://dvc.org)
[![Git](https://img.shields.io/badge/Git-Tracking-orange?logo=git&style=for-the-badge)](https://git-scm.com)
[![Python](https://img.shields.io/badge/Python-3.8+-blue?logo=python&style=for-the-badge)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

> **Master the art of Data Version Control.** This repository serves as a professional blueprint for implementing DVC in production machine learning workflows.

---

## 📖 Overview

In modern MLOps, code versioning is only half the battle. **DVC (Data Version Control)** fills the gap by providing a Git-like experience for data and models. This project demonstrates how to build reproducible, shareable, and scalable ML pipelines.

### Why DVC?

| Feature | Git (Code) | DVC (Data/Models) |
| :--- | :--- | :--- |
| **Storage** | Local + GitHub/GitLab | Local + S3/GCS/Azure/DagsHub |
| **File Size** | Small (<100MB) | Large (GBs/TBs) |
| **Versioning** | Content-based (Diffs) | Content-based (Hashes) |
| **Tracking** | Direct file tracking | Meta-file (`.dvc`) tracking |

---

## 🏗️ System Architecture

DVC operates as a layer on top of Git. It intercepts large files, stores them in a dedicated cache, and provides Git with lightweight "pointer" files.

### The DVC Lifecycle

1.  **Workspace**: Your active development area.
2.  **DVC Cache**: A hidden content-addressable storage (`.dvc/cache`).
3.  **Remote Storage**: The "Source of Truth" for data (e.g., AWS S3).
4.  **Git Index**: Tracks the `.dvc` files that point to specific versions in the cache/remote.

---

## 🚀 Professional Workflow

### 1. Environment Setup
Create a virtual environment and install dependencies:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install dvc[s3] dvc[gdrive]  # Install with desired remotes
```

### 2. Project Initialization
```bash
git init
dvc init
git commit -m "Initialize DVC tracking"
```

### 3. Data Tracking Strategy
Instead of tracking raw files, we track the metadata:
```bash
# Add a dataset
dvc add data/raw_features.csv

# This creates 'data/raw_features.csv.dvc'
git add data/raw_features.csv.dvc .gitignore
git commit -m "Track raw features version 1.0"
```

### 4. Pipeline Orchestration (`dvc.yaml`)
Define stages to ensure reproducibility. DVC will only re-run stages if dependencies change.
```yaml
stages:
  process:
    cmd: python src/process.py data/raw.csv data/processed.csv
    deps:
      - src/process.py
      - data/raw.csv
    outs:
      - data/processed.csv
  train:
    cmd: python src/train.py data/processed.csv model.pkl
    deps:
      - src/train.py
      - data/processed.csv
    outs:
      - model.pkl
```

---

## ☁️ Remote Storage Configuration

DVC supports a wide array of cloud providers. Configuration is stored in `.dvc/config`.

### AWS S3
```bash
dvc remote add -d s3-remote s3://my-ml-bucket/dvc-store
dvc remote modify s3-remote profile my-aws-profile
```

### DagsHub (S3 Compatible)
```bash
dvc remote add -d origin https://dagshub.com/user/repo.dvc
dvc remote modify origin --local auth basic
dvc remote modify origin --local user <username>
dvc remote modify origin --local password <token>
```

---

## 🛠️ Advanced Commands

- **`dvc pull`**: Download data from remote storage.
- **`dvc checkout`**: Sync workspace with the current `.dvc` files.
- **`dvc dag`**: Visualize the pipeline dependency graph.
- **`dvc metrics show`**: Compare experiment results across versions.

---

## 💡 Best Practices

1.  **Never commit data to Git**: Ensure your `.gitignore` is correctly updated by DVC.
2.  **Use Meaningful Tags**: Tag your Git commits (e.g., `v1.0-baseline`) to easily revert data via `git checkout` + `dvc checkout`.
3.  **Automate with CI/CD**: Use CML (Continuous Machine Learning) to run DVC pipelines on every Pull Request.
4.  **Shared Cache**: In team environments, use a shared local cache to avoid redundant downloads.

---

## 🤝 Contributing

We follow the [Contributor Covenant](https://www.contributor-covenant.org/) code of conduct. 

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📜 License

Distributed under the MIT License. See [LICENSE](LICENSE) for more information.

---

<p align="center">
  Developed with ❤️ by <a href="https://github.com/Rupeshbhardwaj002">Rupesh Bhardwaj</a>
</p>
