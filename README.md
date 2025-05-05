
# Compound V2 Wallet Credit Scoring

## Overview

This project builds an AI-powered, decentralized credit scoring system for wallets interacting with the Compound V2 protocol. It processes raw, transaction-level data to assign a credit score between 0 and 100 to each wallet based on behavioral patterns.

Higher scores indicate responsible usage (e.g., repaying loans on time), while lower scores reflect risky or bot-like behaviors (e.g., frequent liquidations, inactivity, or anomalies).

## Objective

Design and implement a scoring system that:
- Processes raw on-chain transaction logs
- Engineers wallet-level behavioral features
- Scores each wallet between 0 and 100
- Produces analytics and interpretable insights for top and bottom scoring wallets

## Data Sources

Raw data from the Compound V2 protocol (public data).
You should select the three largest files from the provided Google Drive folder to ensure representative volume and variety.

## Methodology

1. **Feature Engineering**:  
   Behavioral features such as:
   - First and last transaction time
   - Account age
   - Frequency and consistency of transactions
   - Diversity of actions (deposit, borrow, repay, etc.)

2. **Dimensionality Reduction**:  
   Principal Component Analysis (PCA) is used to capture dominant wallet behavior patterns.

3. **Clustering**:  
   KMeans clustering segments wallets into behavioral groups.

4. **Scoring Logic**:  
   Credit scores are assigned based on cluster characteristics, frequency features, and standard deviations.

## Deliverables

- ✅ `Compound V2 Wallet Scoring.ipynb`: End-to-end pipeline
- ✅ `top_1000_wallet_scores.csv`: Top 1,000 wallets by credit score
- ✅ `Wallet_Analysis_Report.md`: Behavior insights for 5 best and worst wallets
- ✅ `README.md`: Project documentation


## Notes

- This is a self-supervised model using no labels or third-party scoring systems.
- The model is built from scratch without predefined schemas.
- The scoring logic is entirely derived from raw behavior, ensuring uniqueness and interpretability.

---

Built for the Zeru Finance Wallet Scoring Challenge
