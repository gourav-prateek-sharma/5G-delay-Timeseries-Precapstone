# 5G Delay Timeseries Prediction 

## Project Overview 
This project focuses on predicting delay times in 5G networks using time series analysis and machine learning techniques. Our goal is to understand and forecast network performance, which is critical for enhancing the user experience in 5G services.

## Research Findings 
- **Key Insights**:
  - By studying packet traces across the IP, RLC, MAC, and Physical layers, the project identifies the primary cause of the triangular pattern observed in Delay vs. SN (Sequence Number) graphs — Frame Alignment Delay in the RLC layer — and builds predictive models to forecast this behavior.
  - Delay is influenced by various factors including frame alignment , and environmental conditions,.
  - Machine learning models can significantly improve prediction accuracy compared to traditional methods.
  - Analyzed packet traces and built understanding of protocol concepts across IP, RLC, MAC, and Physical layers.
  -Concluded that the main reason for the triangular pattern in Delay vs. SN graphs is due to Frame Alignment Delay in the RLC layer.
  -Constructed and compared various neural network models: RNN, LSTM, and ARIMA.
  -Evaluated prediction strategies: Single-Step, Multi-Step, and a Hybrid approach.
  -Best results were achieved using an LSTM-based Hybrid Multi-Step Recursive approach.

- **Model Performance**:
  - Our hybrid model outperformed traditional time series models by achieving a lower Mean Absolute Error (MAE).
  
## Model Architecture 

The architecture of our hybrid model comprises:
- Time Series Analysis for trend identification.
- Machine Learning algorithms (e.g., Random Forest, XGBoost) for prediction.
- Component	Details
- Model Type	LSTM (Long Short-Term Memory)
- LSTM Units	50
- Output Layer	Dense (1 unit)
- Optimizer	Adam
- Loss Function	Mean Squared Error (MSE)
- Sequence Length	30 timesteps
- Normalization	Min-Max Scaling


┌─────────────────────────────────────────────────────────────┐
│           Hybrid Multi-Step Recursive Prediction            │
│                                                             │
│  Step 1: Feed 30 real (ground truth) values as input        │
│                        │                                    │
│  Step 2: Predict next 10 values recursively                 │
│          → Predict 1st value                                │
│          → Append to sequence, drop oldest                  │
│          → Repeat × 10                                      │
│                        │                                    │
│  Step 3: RESET — use next 30 real values as new input       │
│                        │                                    │
│  Step 4: Repeat for entire dataset                          │
└─────────────────────────────────────────────────────────────┘






| Layer          | Description                                   |
|----------------|-----------------------------------------------|
| Input Layer    | Raw delay data as input                       |
| Feature Layer  | Engineered features from time series data     |
| AI Models      | Multiple ML models for prediction             |
| Output Layer   | Predicted delay values                         |

## Hybrid Approach Explanation 
Time series forecasting model is combined with machine learning techniques to leverage the benefits of both worlds:
- **Time Series**: Captures trends and seasonality in the data.
- **Machine Learning**: Models complex patterns and relationships in the data.
Approach	Problem
Pure Recursive	Error accumulates → phase shift, large drift
Hybrid (this work)	Predicts in small blocks of 10, resets with real data regularly → reduced long-term drift
Key concept: Predict small future blocks → Reset with real data → Repeat.
This approach allows us to enhance our predictions by utilizing historical data trends while applying advanced algorithms.

## Dataset Information 
- **Source**:ExPECA Testbed, KTH Royal Institute of Technology, Sweden
- **Size**: 40,000 records of delay times
- **Features**:IP Delay, RLC layer timing, sequence numbers, packet traces
  - Timestamp: ip.in and ip.out of the recorded delay
  - Delay: Recorded delay in milliseconds
  - Files: TR1.csv (training), TE1.csv (testing)


## Results 
Our model achieved the following metrics on the testing dataset:
- **Mean Absolute Error (MAE)**: 15.5 ms
- **Root Mean Square Error (RMSE)**: 20.3 ms
(on normalization)
| Metric           | Value          |
|------------------|----------------|
| Threshold        | 20.0 ms        |
| Accuracy         | 0.9468(94.68%) |
| precision        | 1              |
| recall           | 0.7765700      |
| RMSE             | 0.441362       |
| R² Score         | 0.9067         |
| MAE              | 0.20087        |
Results Summary
The LSTM-based Hybrid Multi-Step Recursive model outperformed standard RNN, ARIMA, and plain recursive LSTM approaches in long-term forecasting, particularly in:
Reducing prediction phase shift
Minimizing error accumulation over long sequences
Accurately capturing the triangular Delay vs. SN pattern
