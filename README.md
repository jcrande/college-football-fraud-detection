# FraudScoreCFB — College Football Fraud Detection 🏈  

## Overview  
This project introduces an interpretable **Fraud Score** to evaluate college football teams that may be **overrated in national polls** compared to their underlying performance.  

Using data from 2014–2024, the pipeline integrates **game-level stats**, **Sports Reference ratings**, and **standings data** to create a transparent, data-driven measure of whether a team’s poll ranking is justified.  

**Fraud Score (0–100):**  
- **High scores (90–100):** Teams more likely to be “frauds” (over-ranked by polls).  
- **Low scores (0–10):** Teams more likely to be true contenders.  

---

## Features & Methodology  
- **Data Sources:**  
  - [CollegeFootballData](https://collegefootballdata.com/) (game-level)  
  - [Sports Reference](https://www.sports-reference.com/cfb/) (ratings & standings)  

- **Pipeline:**  
  1. Data ingestion and cleaning (games, ratings, standings)  
  2. Team-season aggregates (points, wins, Elo deltas, strength of schedule)  
  3. **Threshold construction:** define elite vs. borderline performance for key metrics  
  4. **Fraud labeling:**  
     - *Fraud (1):* Ranked in AP Top 12 but weak threshold support  
     - *Contender (0):* Ranked in AP Top 12 and strong threshold support  
     - *Irrelevant (2):* Not ranked  
  5. Train Random Forest classifier (fraud vs contender)  
  6. Assign continuous **Fraud Score (0–100)**  

- **Modeling:**  
  - Random Forest (class-weighted, 500 estimators)  
  - Season-aware validation using GroupKFold  
  - Key predictors: MOV, SRS, Elo ratings, scoring efficiency, SOS  

---

## Key Results  
- **Robust predictive performance:**  
  - ROC-AUC ≈ **0.98** (cross-validated across seasons)  
  - Accuracy ≈ **0.93** (season-aware validation)  

- **Interpretability:**  
  - Clear thresholds (e.g., Win% ≥ 0.85, SRS ≥ 18) anchor the labeling process  
  - Fraud Score probability scaling provides a continuous ranking  

- **Sample Insight:**  
  - Teams like *Notre Dame (2014, 2022)* and *Texas (2018, 2019)* frequently score high as “fraud” candidates.  
  - Dominant programs (*Alabama, Georgia, Ohio State*) consistently validate their rankings with low fraud scores.  

---

## Repository Structure  

FraudScoreCFB/

│── FraudScoreCFB.ipynb # Main notebook (pipeline + analysis)

│── data/ # Expected raw CSV files (not included in repo)

│ ├── cfbd_games_2014_2025.csv

│ ├── ratings_YYYY.csv

│ └── standings_YYYY.csv

│── requirements.txt # Dependencies

│── README.md # Project documentation


---

## How to Run  
1. Clone this repo:  
   ```bash
   git clone https://github.com/<your-username>/FraudScoreCFB.git
   cd FraudScoreCFB
Install dependencies:

bash
Copy code
pip install -r requirements.txt
Open the Jupyter Notebook:

bash
Copy code
jupyter notebook FraudScoreCFB.ipynb
Update base_dir in the notebook if your data files are stored elsewhere.

Run cells sequentially to reproduce results.

## Executive Summary
This project demonstrates how statistical thresholds and machine learning can be combined to audit poll-based rankings in college football. The Fraud Score framework not only identifies overrated teams but also provides an interpretable, data-backed method for evaluating ranking integrity.
