# Methodology: Compound V2 Wallet Credit Scoring System

## Overview

This document explains the methodology behind the credit scoring system developed for Zeru Finance to evaluate Compound V2 protocol wallets. The system analyzes wallet transaction histories to assign credit scores between 0 and 100, with higher scores indicating more reliable and responsible protocol usage.

## 1. Data Processing

The system processes raw Compound V2 transaction data from JSON files. Each transaction record is structured to include:

- Transaction type (deposit, borrow, repay, withdraw, liquidation)
- Wallet address
- Transaction timestamp
- Asset information (symbol, ID)
- Amount (raw and USD values)
- Transaction hash and ID
- For liquidations: liquidator address

## 2. Feature Engineering

### Time-Based Features
- **Account Age**: Duration between first and last transaction (days)
- **Transaction Frequency**: Average time between transactions (hours)
- **Temporal Consistency**: Standard deviation of time between transactions

### Transaction-Based Features
- **Activity Volume**: Counts of different transaction types (deposits, borrows, repays, withdraws, liquidations)
- **Financial Volume**: USD values of different transaction types (total, average, maximum)
- **Liquidation History**: Times liquidated, times acting as liquidator
- **Asset Diversity**: Number of unique assets interacted with

### Behavioral Features
- **Repayment Behavior**: Ratio of repayments to borrowings
- **Responsible Borrowing**: Ratio of deposits before borrowing
- **Transaction Patterns**: Detection of deposit→borrow→repay cycles
- **Bot Detection**: Frequency of rapid transaction sequences
- **Position Maintenance**: Negative balance duration, position volatility

## 3. Scoring Methodology

### Positive Indicators (Higher is Better)
| Feature | Weight | Rationale |
|---------|--------|-----------|
| Account Age | 10% | Established accounts demonstrate long-term protocol engagement |
| Repay-to-Borrow Ratio | 15% | High repayment ratios indicate responsible borrowing behavior |
| Deposit-Before-Borrow | 10% | Depositing before borrowing shows responsible planning |
| Repay-After-Borrow | 10% | Timely repayment after borrowing demonstrates reliability |
| Asset Diversification | 5% | Using multiple assets indicates protocol sophistication |
| Transaction Volume | 5% | Active engagement with the protocol (with diminishing returns) |
| Natural Transaction Timing | 5% | Human-like transaction patterns versus automated/bot behavior |

### Negative Indicators (Higher is Worse)
| Feature | Weight | Rationale |
|---------|--------|-----------|
| Liquidation History | 15% | Being liquidated signals failure to maintain healthy collateralization |
| Rapid Transaction Sequences | 5% | Unnaturally quick transaction sequences suggest bot/MEV activity |
| Negative Balance Duration | 10% | Maintaining negative positions indicates risky behavior |
| Position Volatility | 5% | Extreme fluctuations in protocol position suggest unpredictability |
| High Borrow-to-Deposit Ratio | 5% | Excessive borrowing relative to deposits indicates risk-taking |

### Scoring Process

1. **Feature Normalization**: All features are normalized to a 0-1 scale
2. **Initial Score Calculation**: Weighted sum of normalized features
3. **Anomaly Detection**: K-means clustering identifies outlier behavior
4. **Score Adjustments**:
   - Minimum activity threshold penalty
   - Liquidation severity penalty
   - Anomaly detection penalty
5. **Final Scoring**: Adjusted scores are scaled to 0-100 range

## 4. Criteria for "Good" vs "Bad" Wallet Behavior

### "Good" Wallet Behavior
- Consistent deposits before borrowing
- High repayment ratio (near or above 1.0)
- Reasonable borrow-to-deposit ratio
- No liquidations
- Natural timing between transactions
- Long-term protocol engagement
- Diversified asset usage

### "Bad" Wallet Behavior
- Multiple liquidation events
- Low repayment ratio
- Borrowing without prior deposits
- Highly volatile positions
- Suspicious transaction timing (bot-like behavior)
- Exploitative patterns (e.g., flash loans without value addition)
- Limited protocol engagement

## 5. Implementation Details

The scoring system is implemented as a Python data processing pipeline with these stages:

1. Data loading from multiple JSON chunk files
2. Transaction preprocessing and normalization
3. Feature engineering at wallet level
4. Anomaly detection using K-means clustering
5. Score calculation with weighted feature aggregation
6. Final adjustment and output generation

## 6. Evaluation and Validation

The model is validated through:

1. **Manual review** of high and low-scoring wallets
2. **Pattern consistency** across similar behavior profiles
3. **Outlier analysis** to identify edge cases
4. **Distribution analysis** to ensure reasonable score spread

This methodology provides a comprehensive assessment of wallet behavior on Compound V2, focusing on responsible borrowing, repayment reliability, and protocol sustainability.
