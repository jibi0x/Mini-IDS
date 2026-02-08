# Network Intrusion Detection (Mini-IDS)

Mini-IDS is a **flow-based Network Intrusion Detection System (IDS)** that performs real-time analysis of live network traffic using supervised machine learning.

The system captures raw packets from a network interface, aggregates them into network flows, extracts **CICIDS2017-aligned features**, and classifies traffic as **benign or malicious** using a Random Forest model.

The model is trained on a cleaned and feature-selected subset of the **CICIDS2017 dataset**, ensuring consistency between offline training and live inference.

Mini-IDS is designed to run locally on a personal machine and is deployed as a **containerized FastAPI service**, serving as a practical, end-to-end intrusion detection pipeline rather than a dataset-only ML experiment.

---

## Tech Stack

- **Language:** Python  
- **Machine Learning:** Scikit-learn  
- **Model:** Random Forest  
- **Dataset:** CICIDS2017  
- **API:** FastAPI  
- **Deployment:** Docker  
- **Environment:** Local PC (future home lab: Ubuntu Server + Docker)

---

## Project Structure

```text
mini-ids/
├── data/
├── notebooks/
├── training/
├── api/
├── docker/
├── docs/
└── README.md
```

Environment:
- Python 3.10+
- venv

Requirements:  
pandas  
numpy  
scikit-learn  
matplotlib  
seaborn  
fastapi  
uvicorn  
joblib  

---

**Phase 0: Live Traffic Capture & Flow Generation**  
Capture network packets from a local network interface and aggregate them into bidirectional network flows.

Scope:  
- Packet capture using Scapy (or tshark)  
- Flow identification using 5-tuple (src IP, dst IP, src port, dst port, protocol)  
- Flow lifecycle management (timeouts, expiration)  
- Packet and byte counters per flow  

Output:  
- In-memory flow records ready for feature extraction  

---

**Phase 1: Dataset Ingestion & Cleaning**  
Prepare the CICIDS2017 dataset for model training.

Scope:  
- Load CICIDS2017 CSV files  
- Handle missing, infinite, and malformed values  
- Remove irrelevant or redundant columns  
- Normalize labels (binary or multi-class)  
- Ensure consistency across merged datasets  

Output:  
- Cleaned, structured training dataset  

---

**Phase 2: Feature Selection & Schema Definition**  

Define a feature schema that can be reproduced in live traffic.

Scope:  
- Select a subset of CICIDS2017 flow features suitable for live extraction  
- Perform correlation analysis and feature importance ranking  
- Finalize the feature list used by both training and inference  
- Lock feature ordering and data types  

Output:  
- Fixed feature schema shared by training and live inference  

---

**Phase 3: Feature Engineering**  

Transform raw flow data into ML-ready numerical features.

Scope:  
- Compute derived metrics (rates, averages, durations)  
- Encode categorical values (e.g., protocol)  
- Scale or normalize numerical features if required  
- Apply identical transformations to training and live data  

Output:  
- Feature vectors aligned with the model’s input requirements  

---

**Phase 4: Model Training**  

Train a supervised machine learning model for intrusion detection.

Scope:  
- Train/validation/test split  
- Random Forest classifier training  
- Hyperparameter tuning  
- Class imbalance handling  
- Model performance optimization with security-focused metrics  

Output:  
- Trained intrusion detection model  

---

**Phase 5: Model Evaluation & Validation**  

Assess model effectiveness using security-relevant metrics.

Scope:  
- Precision, recall, F1-score  
- Confusion matrix analysis  
- False positive and false negative evaluation  
- ROC-AUC measurement  

Output:  
- Validated model with documented performance  

---

**Phase 6: Model Serialization & Artifact Management**  

Prepare the model for deployment.

Scope:  
- Serialize trained model using joblib  
- Save feature schema and preprocessing artifacts  
- Version trained models for reproducibility  

Output:  
- Deployment-ready model artifacts  

---

**Phase 7: Real-Time Inference Engine**  

Perform real-time intrusion detection on live network traffic.

Scope:  
- Convert expired flows into feature vectors  
- Load trained model into memory  
- Perform real-time predictions  
- Generate detection results (benign vs attack)  

Output:  
- Real-time intrusion classification results  

---

**Phase 8: API Development**  

Expose intrusion detection results through a REST API.

Scope:  
- FastAPI-based inference endpoint  
- Input validation using Pydantic  
- JSON-based prediction responses  
- Localhost deployment for development  

Output:  
- Accessible inference API for live detection  

---

**Phase 9: Containerization & Local Deployment**  

Deploy the Mini-IDS in a reproducible environment.

Scope:  
- Dockerized application stack  
- Local deployment on personal PC  
- Network interface access configuration  
- Preparation for future home lab deployment  

Output:  
- Containerized Mini-IDS running locally  

---

**Phase 10: Documentation & Threat Context**  

Document system behavior and security relevance.

Scope:  
- System architecture documentation  
- Attack categories supported  
- Dataset limitations and assumptions  
- Future improvements (pcap ingestion, SIEM integration, Zeek/Suricata)  

Output:  
- Complete project documentation  