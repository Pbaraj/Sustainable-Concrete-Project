# Predicting the Compressive Strength of Sustainable Concrete with Machine Learning

This project predicts the **compressive strength of sustainable concrete mixes** using machine learning and deep learning. The aim is to estimate concrete strength from mix-design parameters so that engineers can reduce repeated physical testing, save time, and support more efficient sustainable concrete design.

The project was developed for the course **Artificial Intelligence in Engineering**.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Problem Statement](#problem-statement)
- [Dataset](#dataset)
- [Features Used](#features-used)
- [Methodology](#methodology)
- [Models Implemented](#models-implemented)
- [Results](#results)
- [Key Findings](#key-findings)
- [Project Files](#project-files)
- [Installation](#installation)
- [How to Run](#how-to-run)
- [Outputs and Visualizations](#outputs-and-visualizations)
- [Lessons Learned](#lessons-learned)
- [Future Improvements](#future-improvements)
- [Technologies Used](#technologies-used)
- [Author](#author)
- [References](#references)

---

## Project Overview

Concrete compressive strength is one of the most important mechanical properties in civil and structural engineering. Traditionally, it is measured through laboratory testing after casting and curing concrete samples. This process is reliable but can be time-consuming and expensive.

In this project, machine learning models were trained to predict concrete compressive strength using sustainable concrete mix data. The project compares a classical machine learning model with a neural network model:

1. **Random Forest Regressor** as an interpretable baseline model  
2. **PyTorch Feed-Forward Neural Network** as the main deep learning model

The final neural network model achieved strong prediction accuracy and exceeded the target performance requirements.

---

## Problem Statement

The main objective is to predict the compressive strength of sustainable concrete based on mix-design features such as cement, water, aggregates, admixture, curing age, and water/cement ratio.

The target variable is:

```text
Compressive Strength (MPa)
```

The project target was:

```text
RMSE ≤ 3 MPa
R² ≥ 0.92
```

---

## Dataset

The project uses a sustainable concrete dataset containing concrete mix properties and corresponding compressive strength values.

### Original Dataset

```text
Number of samples: 1,000
Number of columns: 10
```

### Dataset Columns

```text
Cement
Water
Admixture
Coarse_Aggregate
M_Sand
Age
Compressive_Strength
Embodied_CO2
Energy_Consumption
Resource_Consumption
```

### Data Quality Check

The dataset was checked for missing values. No missing values were found in any column.

### Outlier Removal

Samples with compressive strength above **200 MPa** were removed because they were treated as unrealistic or extreme outliers for typical concrete applications.

After filtering:

```text
Final dataset size: 545 samples × 10 columns
```

---

## Features Used

The following input features were used for model training:

```text
Cement
Coarse_Aggregate
M_Sand
Water
Admixture
Age
w_c_ratio
```

The feature `w_c_ratio` was engineered using:

```text
w_c_ratio = Water / Cement
```

This feature was added because the water/cement ratio is a well-known factor influencing concrete strength.

---

## Target Transformation

The target variable, compressive strength, was transformed using:

```python
np.log1p(Compressive_Strength)
```

This transformation helped stabilize the target distribution and improved neural network training.

Predictions were later converted back to MPa using:

```python
np.expm1(prediction)
```

---

## Methodology

The project workflow followed these main steps:

1. Load the sustainable concrete dataset
2. Check for missing values
3. Remove unrealistic strength outliers above 200 MPa
4. Engineer the water/cement ratio feature
5. Select relevant input features
6. Transform the target variable using `log1p`
7. Train and evaluate a Random Forest model
8. Train and evaluate a PyTorch Neural Network
9. Compare model performance using RMSE, MAE, and R²
10. Analyze prediction errors and visualize results

---

## Models Implemented

## 1. Random Forest Regressor

The Random Forest model was used as a baseline model because it performs well on tabular data and provides feature importance values.

### Model Settings

```text
Model: RandomForestRegressor
Number of trees: 250
Maximum depth: 12
Random seed: 2025
Cross-validation: 5-fold
```

### Cross-Validation

A 5-fold cross-validation was performed to check model stability.

```text
CV RMSE values: [0.04, 0.04, 0.03, 0.03, 0.03]
Mean CV RMSE: 0.03
Standard deviation: 0.00
```

---

## 2. PyTorch Neural Network

A feed-forward neural network was developed using PyTorch.

### Network Architecture

```text
Input layer: 7 input features
Hidden layer 1: 64 neurons + ReLU
Hidden layer 2: 32 neurons + ReLU
Hidden layer 3: 16 neurons + ReLU
Output layer: 1 predicted value
```

### Training Techniques

The neural network used several techniques to improve stability and avoid overfitting:

```text
Early stopping
Learning rate scheduling
Weight decay
Dropout
Gradient clipping
TensorBoard monitoring
```

### Train/Validation/Test Split

```text
Training set: 70%
Validation set: 15%
Test set: 15%
```

The model stopped training early at epoch 206.

---

## Results

### Model Performance

| Model | RMSE | MAE | R² Score |
|---|---:|---:|---:|
| Random Forest Regressor | 4.98 MPa | 3.94 MPa | 0.971 |
| PyTorch Neural Network | 2.27 MPa | 1.81 MPa | 0.994 |

The **PyTorch Neural Network** achieved the best performance and successfully met the project target.

---

## Key Findings

- The neural network performed better than the Random Forest model.
- The neural network achieved an RMSE of **2.27 MPa** and an R² score of **0.994**.
- The Random Forest model also performed strongly and gave useful feature importance insights.
- **Age** was the most important feature for strength prediction.
- **Cement content** was the second most important feature.
- The engineered **water/cement ratio** contributed to model performance.
- Most predictions were very close to the actual compressive strength values.
- The largest neural network prediction error was approximately **7.44 MPa**, mainly at very high strength values.

---

## Feature Importance

Random Forest feature importance showed the following approximate values:

| Feature | Importance |
|---|---:|
| Age | 0.720 |
| Cement | 0.196 |
| Coarse Aggregate | 0.045 |
| Water/Cement Ratio | 0.021 |
| Water | 0.007 |
| M-Sand | 0.006 |
| Admixture | 0.005 |

This confirms that curing age and cement content are the dominant variables for predicting concrete compressive strength.

---

## Project Files

A suggested GitHub structure for this project is:

```text
project-folder/
│
├── README.md
├── Project 02_03781430.ipynb
├── Technical Report _Project 02_03781430.pdf
├── sustainable_concrete_dataset.csv
├── final_net.pth
│
├── results/
│   ├── feature_importance.png
│   ├── prediction_error_distribution.png
│   ├── neural_network_learning_curve.png
│   └── predicted_vs_actual_strength.png
│
└── requirements.txt
```

### Main Files

| File | Description |
|---|---|
| `README.md` | Project documentation for GitHub |
| `Project 02_03781430.ipynb` | Main Jupyter Notebook containing code, training, and evaluation |
| `Technical Report _Project 02_03781430.pdf` | Final technical report |
| `sustainable_concrete_dataset.csv` | Dataset used for model training |
| `final_net.pth` | Saved PyTorch neural network weights |
| `requirements.txt` | Python dependencies |

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/your-repository-name.git
cd your-repository-name
```

Replace `your-username/your-repository-name` with your actual GitHub repository path.

### 2. Create a Virtual Environment

For Windows:

```bash
python -m venv venv
venv\Scripts\activate
```

For macOS/Linux:

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Required Packages

```bash
pip install -r requirements.txt
```

If a `requirements.txt` file is not available, install the required packages manually:

```bash
pip install pandas numpy torch scikit-learn matplotlib tensorboard jupyter
```

---

## How to Run

### Option 1: Run with Jupyter Notebook

Start Jupyter Notebook:

```bash
jupyter notebook
```

Open and run:

```text
Project 02_03781430.ipynb
```

### Option 2: Run with JupyterLab

```bash
jupyter lab
```

Then open the notebook and run all cells.

### Important Note

The dataset file must be placed in the same directory as the notebook, because the code reads:

```python
datafile = 'sustainable_concrete_dataset.csv'
```

---

## Outputs and Visualizations

The project generates several useful outputs:

### 1. Random Forest Feature Importance Plot

This plot shows which input variables have the strongest influence on compressive strength prediction. Age and cement content are the most influential features.

### 2. Neural Network Learning Curve

The learning curve shows training and validation loss over epochs. It helps confirm whether the neural network is learning properly and whether overfitting is occurring.

### 3. Prediction Error Distribution

The error distribution shows how far the neural network predictions are from the actual compressive strength values.

### 4. Predicted vs Actual Strength Plot

This plot compares predicted compressive strength against actual compressive strength. Points close to the diagonal line indicate accurate predictions.

---

## Example Results from Notebook

```text
Original dataset shape: (1000, 10)
Shape after outlier removal: (545, 10)
```

```text
Random Forest RMSE: 4.98 MPa
Random Forest MAE: 3.94 MPa
Random Forest R²: 0.971
```

```text
Neural Network RMSE: 2.27 MPa
Neural Network MAE: 1.81 MPa
Neural Network R²: 0.994
```

---

## Lessons Learned

- Data cleaning is important, especially when small datasets contain extreme values.
- Domain-based feature engineering improves model quality.
- The water/cement ratio is a useful engineering feature for concrete strength prediction.
- Random Forest is a strong and interpretable baseline for tabular engineering data.
- Neural networks can achieve excellent accuracy when trained with proper validation and regularization.
- TensorBoard monitoring and early stopping help prevent overfitting.

---

## Future Improvements

This project can be extended in several ways:

- Apply hyperparameter tuning using Optuna or GridSearchCV
- Test advanced models such as XGBoost, LightGBM, and CatBoost
- Add SHAP analysis for better interpretability
- Include environmental indicators such as embodied CO₂ in multi-objective prediction
- Deploy the trained model as a Streamlit or FastAPI web application
- Compare predicted results with additional laboratory test data
- Build a user interface where engineers can enter mix properties and receive predicted strength

---

## Technologies Used

```text
Python
Pandas
NumPy
Scikit-learn
PyTorch
TensorBoard
Matplotlib
Jupyter Notebook
```

---

## Author

**Pradyumna Manoj Baraj**  
Technical University of Munich  
Course: Artificial Intelligence in Engineering  
Date: 20.07.2025

---

## References

- Scikit-learn Documentation: https://scikit-learn.org/
- PyTorch Documentation: https://pytorch.org/
- Kaggle Dataset: Sustainable Concrete Dataset

---

## License

This project is intended for academic and educational purposes.
