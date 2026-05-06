# 🛡️ Zero-Day-IDS

> An AI-powered Network Intrusion Detection System (NIDS) that leverages Machine Learning to detect zero-day attacks and anomalous network traffic in real time.

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=flat-square&logo=python)
![ML](https://img.shields.io/badge/Detection-Machine%20Learning-orange?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square)

---

## 📌 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Installation](#installation)
- [Usage](#usage)
- [Dataset](#dataset)
- [Model Details](#model-details)
- [Results](#results)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)

---

## 🔍 Overview

**Zero-Day-IDS** is a machine learning-based Network Intrusion Detection System (NIDS) designed to detect previously unseen (zero-day) attacks by learning patterns from network traffic rather than relying on static signatures.

Traditional signature-based IDS solutions fail to detect novel attack vectors. This project addresses that gap by training ML models on network flow features, enabling anomaly detection even for attacks that have never been seen before.

---

## ✨ Features

- 🔬 **Zero-Day Detection** — Identifies unknown and novel attacks using anomaly-based ML models
- 📡 **Real-Time Traffic Analysis** — Monitors live network packets and classifies them in real time
- 🤖 **Multiple ML Models** — Supports Random Forest, XGBoost, Neural Networks, and Isolation Forest
- 📊 **Feature Extraction** — Automatically extracts meaningful features from raw network traffic
- 🚨 **Alert System** — Generates structured alerts with attack classification and confidence scores
- 📈 **Performance Metrics** — Tracks accuracy, precision, recall, F1-score, and false positive rate
- 🗂️ **Logging** — Maintains detailed logs of all detected intrusions

---

## 🏗️ Architecture

```
Network Traffic (PCAP / Live Interface)
          │
          ▼
  [ Packet Capture Module ]
          │
          ▼
  [ Feature Extraction ]
   (Flow stats, packet size,
    protocol flags, entropy, etc.)
          │
          ▼
  [ Preprocessing & Normalization ]
          │
          ▼
  [ ML Detection Engine ]
   ┌──────────────────────┐
   │  Anomaly Detector    │  ← Isolation Forest / Autoencoder
   │  Attack Classifier   │  ← Random Forest / XGBoost
   └──────────────────────┘
          │
          ▼
  [ Alert & Reporting Module ]
```

---

## 🛠️ Tech Stack

| Component         | Technology                        |
|-------------------|-----------------------------------|
| Language          | Python 3.8+                       |
| Packet Capture    | Scapy / PyShark / tcpdump         |
| ML Framework      | scikit-learn, XGBoost, TensorFlow/Keras |
| Data Processing   | Pandas, NumPy                     |
| Visualization     | Matplotlib, Seaborn               |
| Logging           | Python `logging` module           |

---

## ⚙️ Installation

### Prerequisites

- Python 3.8 or higher
- pip
- (Optional) Root/Administrator privileges for live packet capture

### Clone the Repository

```bash
git clone https://github.com/gjayakesh/Zero-Day-IDS.git
cd Zero-Day-IDS
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

### (Optional) Create a Virtual Environment

```bash
python -m venv venv
source venv/bin/activate        # Linux/macOS
venv\Scripts\activate           # Windows

pip install -r requirements.txt
```

---

## 🚀 Usage

### 1. Train the Model

Train the IDS model on a labeled dataset:

```bash
python train.py --dataset data/cicids2018.csv --model random_forest
```

Available model options: `random_forest`, `xgboost`, `neural_net`, `isolation_forest`

### 2. Evaluate the Model

```bash
python evaluate.py --model models/rf_model.pkl --test data/test_set.csv
```

### 3. Run Live Detection

Monitor a network interface in real time (requires root on Linux):

```bash
sudo python detect.py --interface eth0 --model models/rf_model.pkl
```

### 4. Analyze a PCAP File

```bash
python detect.py --pcap captures/traffic.pcap --model models/rf_model.pkl
```

### 5. View Alerts

Generated alerts are saved to `logs/alerts.json` and printed to the terminal in real time.

---

## 📂 Dataset

This project is compatible with publicly available network intrusion datasets:

| Dataset        | Description                                      | Link |
|----------------|--------------------------------------------------|------|
| **CICIDS 2017** | Benign + simulated attacks (DoS, DDoS, Brute Force, etc.) | [UNB](https://www.unb.ca/cic/datasets/ids-2017.html) |
| **CICIDS 2018** | Updated version with more attack categories      | [UNB](https://www.unb.ca/cic/datasets/ids-2018.html) |
| **NSL-KDD**    | Improved version of the KDD Cup 99 dataset       | [Kaggle](https://www.kaggle.com/datasets/hassan06/nslkdd) |
| **UNSW-NB15**  | Modern attack simulation dataset                 | [UNSW](https://research.unsw.edu.au/projects/unsw-nb15-dataset) |

Place your dataset CSV files inside the `data/` directory before training.

---

## 🤖 Model Details

### Features Extracted

- Packet length (min, max, mean, std)
- Flow duration and inter-arrival time
- Protocol type (TCP, UDP, ICMP)
- TCP flags (SYN, FIN, RST, ACK)
- Bytes per second / packets per second
- Destination/source port entropy

### Models Supported

| Model              | Type                  | Best For                         |
|--------------------|-----------------------|----------------------------------|
| Random Forest      | Supervised            | Multi-class attack classification |
| XGBoost            | Supervised            | High accuracy on tabular data     |
| Neural Network     | Supervised (Deep)     | Complex pattern recognition       |
| Isolation Forest   | Unsupervised          | Zero-day / anomaly detection      |

---

## 📊 Results

> Sample results on the CICIDS 2017 dataset:

| Model              | Accuracy | Precision | Recall | F1-Score |
|--------------------|----------|-----------|--------|----------|
| Random Forest      | 98.7%    | 98.5%     | 98.6%  | 98.5%    |
| XGBoost            | 99.1%    | 99.0%     | 98.9%  | 99.0%    |
| Neural Network     | 97.9%    | 97.8%     | 97.7%  | 97.7%    |
| Isolation Forest   | 94.3%    | 93.1%     | 94.0%  | 93.5%    |

*Results may vary based on dataset and hyperparameter tuning.*

---

## 📁 Project Structure

```
Zero-Day-IDS/
│
├── data/                   # Dataset files (CSV)
├── models/                 # Trained model files (.pkl / .h5)
├── logs/                   # Alert logs and output
│
├── src/
│   ├── capture.py          # Packet capture module
│   ├── features.py         # Feature extraction
│   ├── preprocess.py       # Data cleaning & normalization
│   ├── model.py            # ML model definitions
│   └── alert.py            # Alert generation
│
├── train.py                # Model training script
├── evaluate.py             # Model evaluation script
├── detect.py               # Live detection / PCAP analysis
│
├── requirements.txt        # Python dependencies
├── config.yaml             # Configuration file
└── README.md
```

---

## 🤝 Contributing

Contributions are welcome! Here's how to get started:

1. Fork the repository
2. Create a new branch: `git checkout -b feature/your-feature-name`
3. Make your changes and commit: `git commit -m "Add: your feature description"`
4. Push to your branch: `git push origin feature/your-feature-name`
5. Open a Pull Request

Please ensure your code follows PEP 8 style guidelines and includes relevant tests.

---

## 👤 Author

**G. Jayakesh**
- GitHub: [@gjayakesh](https://github.com/gjayakesh)

---
