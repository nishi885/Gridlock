# Gridlock: Traffic Demand Prediction

A machine learning project to predict traffic demand across different locations using gradient boosting models.

## 📋 Project Overview

This project develops an end-to-end pipeline for traffic demand forecasting using real-world traffic data. The goal is to predict traffic demand for various locations based on temporal patterns, geographical features, and infrastructure characteristics.

### Key Objectives
- Build a robust demand prediction model
- Analyze key drivers of traffic demand
- Compare multiple state-of-the-art boosting algorithms
- Achieve strong predictive performance for traffic forecasting

---

## 🎯 Problem Statement

Traffic demand prediction is essential for:
- **Traffic Management**: Optimizing traffic flow and reducing congestion
- **Resource Allocation**: Efficient deployment of traffic enforcement
- **Urban Planning**: Infrastructure development decisions
- **Ride-Sharing Services**: Dynamic routing and vehicle positioning

The challenge is to accurately forecast traffic demand while capturing complex nonlinear patterns and temporal/geographic dependencies.

---

## 📊 Dataset

### Data Structure
- **Training Data**: `dataset/train.csv` - Historical traffic data with engineered features
- **Training Features**: `dataset/train_fe.csv` - Feature-engineered training dataset
- **Test Data**: `dataset/test.csv` - Test set for predictions
- **Submission Format**: `dataset/sample_submission.csv` - Expected output format

### Key Features
- **Temporal Features**: Hour of day, day of week, peak hour indicators
- **Geographic Features**: Geohash, road type, number of lanes
- **Infrastructure Features**: Large vehicle allowance, landmark presence
- **Environmental Features**: Temperature
- **Target Variable**: Demand (normalized traffic volume)

---

## 🔧 Model Development

### Why Tree-Based Models?

Gradient boosting models were selected for their advantages:
- **Nonlinear Relationships**: Handle complex traffic demand patterns
- **Tabular Data**: Excellent performance on structured data
- **Feature Scaling**: Robust to unscaled features
- **Feature Interactions**: Capture complex dependencies
- **Interpretability**: Feature importance rankings available

---

## 🤖 Models Explored

### 1. XGBoost Model ⭐ **BEST**

**Configuration:**
```python
n_estimators = 500
learning_rate = 0.05
max_depth = 35
```

**Strengths:**
- Excellent handling of nonlinear patterns
- Captures complex feature interactions
- Strong regularization capabilities

**Performance:**
- **Validation R² = 0.91** ✓
- Best performing model across all metrics

---

### 2. LightGBM Model

**Configuration:**
```python
n_estimators = 1000
learning_rate = 0.05
max_depth = 8
num_leaves = 100
```

**Strengths:**
- Fast training speed
- Memory efficient
- Suitable for large-scale datasets

**Performance:**
- Validation R² = 0.82

---

### 3. CatBoost Model

**Configuration:**
```python
iterations = 2000
learning_rate = 0.03
depth = 10
loss_function = RMSE
eval_metric = R2
```

**Strengths:**
- Native categorical feature handling
- Strong performance on mixed-type data
- Reduced preprocessing requirements

**Performance:**
- Validation R² = 0.72

---

## 📈 Model Comparison

| Model | R² Score | Key Feature |
|-------|----------|-------------|
| **XGBoost** | **0.91** | Best generalization, complex patterns |
| LightGBM | 0.82 | Fast training, memory efficient |
| CatBoost | 0.72 | Categorical handling, native support |

### Key Observation
**XGBoost** demonstrated the strongest generalization capability and achieved the highest validation score (R² = 0.91), making it the best model for deployment.

---

## 🎯 Feature Importance Insights

### Top Important Features

| Feature | Importance Score |
|---------|------------------|
| Temperature | 47,660 |
| Hour | 12,099 |
| Hour (Cosine Transform) | 8,054 |
| Hour (Sine Transform) | 8,024 |
| Number of Lanes | 3,485 |
| Weather Encoding | 1,666 |
| Is Weekend | 1,510 |
| Has Landmark | 1,497 |
| Road Type Encoding | 1,221 |
| Is Peak Hour | 1,141 |

### Key Findings

Traffic demand is primarily driven by:

1. **Environmental Factors**
   - Temperature (47,660) - Strongest predictor
   - Weather conditions (1,666)

2. **Temporal Patterns**
   - Hour of day (12,099)
   - Cyclical hour patterns (Sine/Cosine: ~16k combined)
   - Peak hour indicators (1,141)
   - Weekend vs. weekday (1,510)

3. **Infrastructure Characteristics**
   - Number of lanes (3,485)
   - Road type (1,221)

4. **Geographic Factors**
   - Geohash information (implied through location encoding)
   - Landmark presence (1,497)

5. **Vehicle Composition**
   - Large vehicle allowance indicators

### Notable Observation
Weather and landmarks showed relatively lower individual influence compared to infrastructure and temporal factors.

---

## 🔄 Final Pipeline

```
Raw Data
    ↓
Data Cleaning
    ↓
Missing Value Handling
    ↓
Feature Engineering
    ├── Temporal features (hour, day, cyclical encoding)
    ├── Geographic features (geohash processing)
    └── Infrastructure features (road type, lanes)
    ↓
Encoding
    ├── Categorical encoding
    └── Feature normalization
    ↓
Train/Validation Split
    ├── Training set (80%)
    └── Validation set (20%)
    ↓
Model Training
    ├── XGBoost
    ├── LightGBM
    └── CatBoost
    ↓
Model Evaluation (R² Score)
    ↓
Best Model Selection (XGBoost)
    ↓
Demand Prediction & Submission
```

---

## 📁 Project Structure

```
Gridlock/
├── README.md                          # This file
├── .gitignore
│
├── dataset/
│   ├── train.csv                      # Original training data
│   ├── train_fe.csv                   # Feature-engineered training data
│   ├── test.csv                       # Test data for predictions
│   └── sample_submission.csv          # Expected submission format
│
├── notebooks/
│   ├── person1_eda.ipynb              # Exploratory Data Analysis
│   └── feature_engineer.ipynb         # Feature Engineering Notebook
│
├── scripts/
│   └── run_pipeline.py                # Main pipeline execution script
│
├── output/
│   └── [Generated predictions]        # Model outputs and predictions
│
└── catboost_info/
    ├── catboost_training.json         # CatBoost training logs
    ├── learn_error.tsv                # Training error metrics
    ├── time_left.tsv                  # Training time information
    └── learn/
        └── events.out.tfevents        # TensorFlow event logs
```

---

## 🚀 Getting Started

### Prerequisites
- Python 3.7+
- Required Libraries:
  ```
  pandas
  numpy
  scikit-learn
  xgboost
  lightgbm
  catboost
  matplotlib
  seaborn
  ```

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/nishi885/Gridlock.git
   cd Gridlock
   ```

2. Create a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

### Running the Pipeline

Execute the complete training and prediction pipeline:
```bash
python scripts/run_pipeline.py
```

This will:
1. Load and clean the data
2. Engineer features
3. Train all three models
4. Generate predictions
5. Output results to `output/` directory

### Exploratory Analysis

Run the Jupyter notebooks to explore the data and feature engineering:
```bash
jupyter notebook notebooks/person1_eda.ipynb
jupyter notebook notebooks/feature_engineer.ipynb
```

---

## 📊 Results & Achievements

### Model Performance
- **Best Model**: XGBoost with R² = 0.91
- **Validation Accuracy**: Excellent generalization on unseen data
- **Prediction Coverage**: All test instances successfully predicted

### Key Achievements

✅ **End-to-End Pipeline**: Built a complete demand prediction workflow from raw data to predictions

✅ **Feature Engineering**: Created meaningful temporal and geographical features:
   - Cyclical encoding for hour-of-day patterns
   - Geohash-based geographic clustering
   - Temporal indicators (peak hours, weekends)

✅ **Model Comparison**: Evaluated three state-of-the-art gradient boosting algorithms with comprehensive analysis

✅ **Strong Predictive Performance**: Achieved R² = 0.91 for traffic demand forecasting

✅ **Interpretability**: Identified key demand drivers through feature importance analysis

---

## 📈 Model Insights

### Temporal Patterns
- **Peak Demand Hours**: 11:00-13:00 (lunch hours)
- **Low Demand Hours**: 19:00-20:00 (evening dip)
- **Temperature Dependency**: Strong positive correlation with demand

### Geographic Insights
- **Top Location**: op804a (highest average demand)
- **Infrastructure Impact**: Roads with 4-5 lanes show significantly higher demand
- **Road Type Effect**: Highways have 3x higher demand than residential roads

### Infrastructure Implications
- **Large Vehicle Policy**: Routes allowing large vehicles show ~60% higher demand
- **Lane Capacity**: More lanes correlate with higher traffic volumes

---

## 🔮 Future Improvements

- **Ensemble Methods**: Combine XGBoost, LightGBM, and CatBoost predictions
- **Temporal Validation**: Time-series cross-validation for better temporal generalization
- **Additional Features**: 
  - Historical demand patterns
  - Special events and holidays
  - Traffic incident data
- **Real-Time Predictions**: Deploy as API service for live demand forecasting
- **Deep Learning**: Explore LSTM/Transformer models for sequence patterns

---

## 📝 References

- [XGBoost Documentation](https://xgboost.readthedocs.io/)
- [LightGBM Documentation](https://lightgbm.readthedocs.io/)
- [CatBoost Documentation](https://catboost.ai/)
- Gradient Boosting for Regression: Statistical Learning Theory and Applications

---

## 👥 Authors

- Team: Gridlock Traffic Demand Prediction
- Date: 2026
- Repository: [nishi885/Gridlock](https://github.com/nishi885/Gridlock)

---

## 📄 License

This project is provided as-is for educational and research purposes.

---

**Status**: ✅ Complete & Production Ready
