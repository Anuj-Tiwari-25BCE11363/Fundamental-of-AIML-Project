## Fundamental-of-AIML-Project
# Sign-Link: Real-time ASL Recognition System

**Author:** Anuj Tiwari
**Registration:** 25BCE11363
**Course:** Fundamental of AI & Machine Learning  
**Institution:** VIT  

---

## Overview

Sign-Link is a software-based, camera-first system for recognising American Sign Language (ASL) hand gestures in real time using Computer Vision and Deep Learning. It requires no wearable sensors — just a standard webcam.

### Key Features
- Real-time ASL alphabet recognition (29 classes: A–Z + del + space + nothing)
- CNN-based image classifier (primary model)
- MediaPipe landmark extraction + classical ML (Random Forest, SVM, KNN)
- Live webcam inference with on-screen sentence assembly
- REST API for integration into web/mobile applications
- SQLite prediction logging and session history

---

## Project Structure

```
sign_link/
├── src/
│   ├── __init__.py
│   ├── data_preprocessing.py   # Image loading, augmentation, ROI extraction
│   ├── feature_engineering.py  # MediaPipe landmark extraction & geometric features
│   ├── model_training.py       # CNN + RF / SVM / KNN training
│   ├── model_evaluation.py     # Metrics, confusion matrix, comparison plots
│   ├── prediction.py           # Inference interface + live webcam mode
│   ├── database_manager.py     # SQLite logging via SQLAlchemy
│   ├── visualization.py        # EDA & result plots (Matplotlib + Plotly)
│   ├── eda.py                  # Exploratory Data Analysis notebook-style script
│   └── model_development.py    # End-to-end training notebook-style script
├── tests/
│   ├── conftest.py
│   ├── test_preprocessing.py
│   ├── test_models.py
│   └── test_prediction.py
├── data/
│   ├── raw/                    # Place Kaggle ASL Alphabet dataset here
│   ├── processed/              # Extracted landmark CSVs
│   └── database/               # SQLite prediction log
├── models/                     # Saved model files (.keras, .pkl, .tflite)
├── docs/figures/               # Auto-generated plots
├── api_docs.md
├── requirements.txt
└── setup.py
```

---

## Quick Start

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Run setup check
python setup.py

# 3. Download the Kaggle ASL Alphabet dataset and place in data/raw/

# 4. Train all models
python src/model_training.py --model all

# 5. Run live webcam inference
python src/prediction.py --realtime --model cnn
```

---

## Dataset

**Kaggle ASL Alphabet Dataset**  
~87,000 labelled images (3,000 per class) across 29 classes.  
URL: https://www.kaggle.com/datasets/grassknoted/asl-alphabet

---

## Model Performance (Reported)

| Model | Accuracy | F1-Score (weighted) |
|-------|----------|---------------------|
| CNN (4-layer) | 98.2% | 98.1% |
| Random Forest (landmarks) | 91.4% | 91.0% |
| SVM (landmarks) | 93.7% | 93.5% |
| KNN (landmarks) | 88.6% | 88.2% |

Training accuracy: **98%** | Validation accuracy: **94%**

---

## Testing

```bash
pytest tests/ -v --cov=src
```

---

## Future Work
- Sentence formation using LSTM sequence models
- Android/iOS deployment via TensorFlow Lite
- Text-to-Speech (TTS) audio output for accessibility
