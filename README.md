# AeroPulse India

> An interactive airfare prediction and booking optimization tool for Indian domestic flights, powered by machine learning ($R^2 = 96.8\%$) and calibrated with 2024–2026 domestic pricing trends.

[![Live Demo](https://img.shields.io/badge/Live_Website-Open_App-2ea44f?style=flat-square)](https://devadathan-a.github.io/indian-airfare-optimization-engine/)
[![Python](https://img.shields.io/badge/Python-3.10+-3776ab?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-f7df1e?style=flat-square&logo=javascript&logoColor=black)](https://developer.mozilla.org/)
[![Scikit-Learn](https://img.shields.io/badge/scikit--learn-1.3+-f7931e?style=flat-square&logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](LICENSE)

[**Try the Live Website**](https://devadathan-a.github.io/indian-airfare-optimization-engine/) • [Explore Code](#repository-structure) • [View Model Benchmark](#model-performance)

---

## About the Project

Flight prices in India change constantly based on how close you are to your travel date, what time of day you fly, and airline demand surges. Most public flight price prediction models use older datasets that don't include newer airlines like Akasa Air or current fuel and tariff trends.

I built **AeroPulse India** to create a more realistic, practical flight search and airfare valuation tool. It calculates fair flight prices across 89 commercial Indian airports, compares them against dynamic spot prices, and advises travelers whether to book now or wait for a price drop.

The entire web application runs directly in the browser with zero server requirements, making it completely free to host, fast to load, and accessible on any device via GitHub Pages.

---

## What It Does

- **89 Indian Airports:** Complete directory from metro hubs (Delhi, Mumbai, Bengaluru) to regional and island airports (Ayodhya, Itanagar, Port Blair, Agatti Island) with exact distance and duration math.
- **Real-Time Fair Value Benchmark:** Evaluates flights against an ensemble ML model ($R^2 = 96.8\%$) to flag whether a fare is a **Great Deal**, **Fair Price**, or **Surge Inflated**.
- **Buy vs. Wait Decision Advisor:** Calculates a price drop probability based on your travel date and days remaining before departure.
- **30-Day Fare Calendar:** An interactive chart that maps out daily minimum airfares over the upcoming month to help spot cheaper travel windows.
- **Hourly Price Dynamics:** Visualizes how flight prices vary across departure times (early morning, peak morning, afternoon saver, evening, and red-eye).
- **Type-Ahead Search:** A fast search box with keyboard navigation that lets you quickly search by city, airport name, or 3-letter IATA code (e.g., `DEL`, `BOM`, `AYJ`).

---

## How It Works

1. **Flight Physics (Haversine Formula):**  
   Every airport's GPS coordinates are stored locally. When you select an origin and destination, the app uses the Haversine formula to compute the Great-Circle aerial distance and estimates flight duration based on commercial jet cruising speeds (~720 km/h) plus climb and approach buffers.

2. **Dynamic Spot Pricing:**  
   Models real-world airline yield management principles, applying realistic adjustments for booking proximity (e.g., last-minute spikes vs. early-bird discounts), peak departure times, and weekend demand.

3. **Machine Learning Valuation Engine:**  
   The underlying machine learning model evaluates the route, airline, cabin class, duration, and stops to determine the statistical fair price. If the dynamic ticket price is significantly higher than expected, the system alerts you that surge pricing is active.

---

## Model Performance

The model was developed and tested on an 85/15 train-test split using **296,204 validated domestic flight records**:

| Model Architecture | $R^2$ Score | RMSE | Role |
| :--- | :---: | :---: | :--- |
| Linear Regression (Baseline) | 82.48% | ₹8,939 | Linear baseline comparison |
| HistGradientBoosting (Log-transformed) | 96.25% | ₹5,188 | Core non-linear regressor |
| Decision Tree Regressor | 97.34% | ₹4,366 | Sub-tree regressor |
| **Champion Weighted Ensemble** | **96.79%** | **₹4,798** | **Final model deployed** |

### Key Preprocessing Steps
- **Categorical Multi-Hot Encoding:** Applied to `airline`, `source_city`, `destination_city`, and `departure_time`.
- **Target Transformation:** Applied natural logarithmic transformation ($\ln(\text{price})$) to handle skewness and stabilize variance across high business-class fares.
- **Standard Scaling:** Normalized continuous numerical features (`duration`, `days_left`, `stops`).

---

## Airline Fleet Coverage

The project reflects the current market structure of domestic civil aviation in India:

| Airline | Code | Type | Primary Fleet | Approx. Market Share |
| :--- | :---: | :---: | :--- | :---: |
| **IndiGo** | `6E` | Budget / LCC | Airbus A320neo / A321neo | ~62% |
| **Air India** | `AI` | Full Service | A320neo / Boeing 787 Dreamliner | ~14% |
| **Vistara** | `UK` | Full Service | A320neo / A321neo / B787-9 | ~10% |
| **Akasa Air** | `QP` | Budget / LCC | Boeing 737 MAX 8 | ~5% |
| **Air India Express** | `IX` | Value Budget | Boeing 737-800 / A320neo | ~5% |
| **SpiceJet** | `SG` | Budget / LCC | Boeing 737-800 / Q400 | ~4% |

---

## Tech Stack

- **Frontend:** Semantic HTML5, Vanilla CSS3 (dark theme, glassmorphic card design), Modern JavaScript (ES6+).
- **Charts:** [Chart.js](https://www.chartjs.org/) for interactive graphs.
- **Machine Learning & Data:** Python 3.10+, Scikit-learn, Pandas, NumPy, Joblib.
- **Deployment:** GitHub Pages (Static client-side hosting, zero backend server required).

---

## Running Locally

### Option 1: Open Directly in Browser (No Setup Required)
1. Clone the repository:
   ```bash
   git clone https://github.com/Devadathan-A/indian-airfare-optimization-engine.git
   cd indian-airfare-optimization-engine
   ```
2. Double-click `index.html` (or right-click and open with Chrome, Edge, Brave, or Firefox).

### Option 2: Run Python Retraining Script (Optional)
If you want to inspect or retrain the model locally:
1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Run the retraining script:
   ```bash
   python train_realtime_model.py
   ```

---

## Repository Structure

```
indian-airfare-optimization-engine/
├── index.html               # Main user interface
├── style.css                # Dark theme responsive styling
├── app.js                   # Client-side pricing engine and airport directory
├── requirements.txt         # Python dependencies for ML experiments
├── realtime_engine.py       # Python flight intelligence module
├── train_realtime_model.py  # Model retraining pipeline
├── .gitignore               # Standard git ignore rules
├── LICENSE                  # MIT License
├── README.md                # Project documentation
└── models/                  # Serialized ML model weights
    ├── realtime_flight_model.pkl
    ├── realtime_dt_model.pkl
    ├── realtime_scaler.pkl
    ├── realtime_columns.pkl
    └── realtime_metrics.json
```

---

## License

This project is open-source and available under the [MIT License](LICENSE).
