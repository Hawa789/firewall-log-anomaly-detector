# AI-Powered Firewall Log Anomaly Detector

A Python machine-learning project that analyzes firewall traffic logs and identifies potentially suspicious network activity using **Isolation Forest**, feature engineering, and risk-level classification.

## Project Resources

### Interactive Presentation
- [View PowerPoint Presentation](https://view.genially.com/6acaaabddab714c24c7be503)

### Implementation
- [Open Google Colab Notebook](https://colab.research.google.com/drive/1PjqNbinaJWzNe0cBxy0eP-qr5yMMOAaI?usp=sharing)
  

## Project Overview

Firewalls generate network logs that can contain normal traffic as well as potentially suspicious events. Manually reviewing large volumes of logs can make it difficult to identify unusual patterns efficiently.

This project demonstrates a machine-learning approach to firewall log analysis. It processes network traffic data, extracts relevant features, detects potential anomalies, and assigns risk levels to support further investigation.

The goal is to help security analysts identify activity that deserves attention—not to replace professional security analysis or automatically confirm that an attack has occurred.

Link : https://colab.research.google.com/drive/1PjqNbinaJWzNe0cBxy0eP-qr5yMMOAaI?usp=sharing

## Key Features

- Firewall log preprocessing and data quality checks.
- Network traffic feature engineering.
- Anomaly detection using the Isolation Forest algorithm.
- Feature scaling and model persistence.
- Classification of detected events into risk levels.
- Export of analyzed logs and detection results to CSV.
- Functional tests using normal and suspicious traffic examples.

## Project Workflow

```text
Firewall Traffic Logs
         |
         v
Data Inspection and Preprocessing
         |
         v
Network Feature Engineering
         |
         v
Feature Scaling
         |
         v
Isolation Forest Model
         |
         v
Normal Traffic / Potential Anomaly
         |
         v
Risk-Level Classification
         |
         v
Export Results and Review Alerts
```

## Dataset Overview

The dataset used in this experiment contains **5,250 firewall log records**.

| Dataset characteristic | Reported value |
|---|---:|
| Total records | 5,250 |
| Normal records | 5,000 |
| Anomalous records | 250 |
| Original fields | 11 |
| Missing values | 0 |
| TCP records | 3,903 |
| UDP records | 1,347 |

The initial dataset was inspected for missing values and basic traffic characteristics before model preparation.

### Network Features

Feature engineering was used to extract information that could help identify unusual traffic patterns. The engineered features include:

- **Hour:** the hour associated with a network event.
- **Bytes per packet:** a measure of the amount of data transferred relative to packet count.
- **Packet rate:** an indicator of traffic intensity.
- **Failed connection ratio:** a measure related to unsuccessful connection attempts.
- **Unusual port indicator:** a feature that flags selected ports of interest.

The analysis also considered ports such as `21`, `23`, `445`, `3389`, `4444`, `5555`, `8080`, and `9999`. These ports are not inherently malicious; their significance depends on the service, environment, and surrounding traffic.

## Methodology

### 1. Data preprocessing

The firewall logs were inspected and prepared for analysis. The original dataset contained no missing values, according to the project's checks.

### 2. Feature engineering

Network-related features were created to provide the model with additional signals about traffic volume, connection behavior, event timing, and port usage.

### 3. Feature scaling

A fitted scaler was used to prepare numerical features for model processing. The saved scaler can be reused when applying the same preprocessing steps to compatible future data.

### 4. Anomaly detection with Isolation Forest

The project uses **Isolation Forest**, an unsupervised machine-learning algorithm designed to identify observations that differ from the broader data distribution.

Reported model configuration:

| Parameter | Value |
|---|---|
| Algorithm | Isolation Forest |
| Number of estimators | 200 |
| Contamination setting | 0.05 |
| Detection approach | Unsupervised anomaly detection |

The contamination setting is a model parameter that influences the expected proportion of anomalies. It is not a guarantee that the same proportion of actual malicious events will be detected.

### 5. Risk-level assignment

After anomaly detection, the project assigns risk levels to the records. These labels help distinguish routine traffic from events that may require additional inspection.

The risk level is a classification produced by the project's logic; it should not be interpreted as definitive proof that a record represents a real cyberattack.

## Results

### Anomaly Detection Results

The model produced the following results on the dataset used in the experiment.

| Detection outcome | Number of records | Percentage |
|---|---:|---:|
| Classified as normal | 4,987 | 94.99% |
| Flagged as anomalous | 263 | 5.01% |
| **Total analyzed** | **5,250** | **100%** |

### Risk-Level Distribution

| Risk level | Number of records |
|---|---:|
| Low | 4,987 |
| Medium | 13 |
| High | 0 |
| Critical | 250 |
| **Total** | **5,250** |

The 263 flagged records consist of 250 records classified as Critical and 13 classified as Medium. The remaining 4,987 records were classified as Low risk.

These figures describe the output of this experiment. They should not be interpreted as verified real-world attack detection rates without a separate evaluation against reliable ground-truth labels.

## Functional Testing

Targeted tests were performed to check whether the detection pipeline could distinguish selected traffic examples.

| Test scenario | Expected behavior | Reported outcome |
|---|---|---|
| Normal traffic example | Classified as Normal | Passed |
| Suspicious traffic example | Flagged as an Anomaly | Passed |

These checks demonstrate basic functional behavior on the tested examples. They do not, by themselves, establish the model's precision, recall, or performance on unseen operational firewall logs.

## Generated Files

The project produces saved model artifacts and an output dataset.

| File | Purpose |
|---|---|
| `firewall_isolation_forest.pkl` | Saved Isolation Forest model |
| `firewall_scaler.pkl` | Saved feature scaler |
| `firewall_anomaly_detection_results.csv` | CSV containing the detection results |

The exported results file contains **21 columns**, including the original information and additional fields generated during processing and analysis.

## Technology Stack

- **Language:** Python
- **Data processing:** pandas, NumPy
- **Machine learning:** scikit-learn
- **Anomaly detection:** Isolation Forest
- **Feature preprocessing:** feature scaling and feature engineering
- **Model persistence:** joblib or pickle-based model files, according to the notebook implementation
- **Development environment:** Google Colab
- **Results export:** CSV

## How to Run

1. Open the project's Google Colab notebook.
2. Run the cells in order, starting with imports and dataset preparation.
3. Execute the feature-engineering and preprocessing steps.
4. Train or load the Isolation Forest model as implemented in the notebook.
5. Run the anomaly detection and risk-level assignment steps.
6. Inspect the generated results and export the output CSV.

**Notebook:** https://colab.research.google.com/drive/1PjqNbinaJWzNe0cBxy0eP-qr5yMMOAaI?usp=sharing

## Limitations

- Anomaly detection identifies unusual patterns, not necessarily malicious activity.
- Results depend on the dataset, engineered features, scaling method, and model configuration.
- The contamination setting influences how many observations may be flagged.
- The dataset composition may not reflect the traffic patterns of a real enterprise network.
- The model needs evaluation against reliable labels to establish detection quality.
- False positives can require additional investigation, while subtle threats may remain undetected.
- Successful functional tests do not replace a comprehensive security evaluation.
- The prototype should not be treated as a production intrusion detection or prevention system.

## Future Improvements

- Evaluate performance on a larger and more diverse firewall dataset.
- Measure precision, recall, F1-score, and false-positive rate using reliable labels.
- Investigate flagged records to understand the causes of anomalies.
- Compare Isolation Forest with additional anomaly detection methods.
- Tune model parameters and evaluate the effect of the contamination setting.
- Add visualizations for anomaly distributions, traffic behavior, and risk levels.
- Test the workflow on new data while ensuring consistent feature preprocessing.
- Explore integration with a monitoring dashboard after validating performance and security requirements.

## Responsible Security Use

This project is an educational machine-learning prototype for network log analysis. Its alerts should be treated as indicators for investigation rather than proof of malicious behavior. In an operational environment, security analysts should verify suspicious records, consider network context, and use established incident-response procedures before taking action.

## Author

**Hawa Doumbé Traoré**  
GitHub: [@Hawa789](https://github.com/Hawa789)


