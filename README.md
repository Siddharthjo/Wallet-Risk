# Wallet-Risk
This project provides a simple risk scoring system for wallets interacting with the Compound V2/V3 lending protocol.

✅ 1. Data Collection Method

For this assignment, transaction-level data was simulated to replicate wallet interactions with the Compound V2/V3 protocol. In a production-grade pipeline, this data would be programmatically retrieved using:
	•	The Graph Protocol (Compound Subgraph) for borrow, repay, supply, liquidation, and collateral activity.
	•	Etherscan API / Alchemy / Moralis for on-chain transaction histories and wallet-level interactions.

This mock simulation allows testing of the scoring framework while providing a scalable structure for integrating real APIs.

✅ 2. Feature Selection Rationale

To capture the risk profile of a wallet, we engineered five key features:

Feature	Description	Risk Implication
total_borrows	Total borrowed amount from Compound	High borrow with low repay → risk
total_repaid	Total amount repaid	Low repayment → high risk
collateral_ratio	Ratio of collateral posted to borrowed amount	Low ratio → undercollateralized → high risk
liquidation_events	Number of times a wallet was liquidated	Higher frequency → high risk
tx_frequency	Number of interactions with Compound	Low activity → idle/dormant → higher uncertainty

✅ 3. Scoring Method

A risk score was computed using normalized features and a weighted aggregation:

Feature Normalization

Each feature was scaled using Min-Max normalization, ensuring equal contribution from different ranges.

Risk Score Formula

We used the following weighted formula:

risk_score_raw = 
    0.4 * normalized_liquidation_events +
    0.25 * (1 - normalized_collateral_ratio) +
    0.20 * (1 - normalized_repay_ratio) +
    0.15 * (1 - normalized_tx_frequency)

The final risk score (0 to 1000) was calculated as:

final_score = (1 - risk_score_raw) * 1000

This ensures that wallets with higher liquidation counts, low collateral, and low repayment ratios receive lower scores, indicating higher risk.


✅ 4. Justification of Risk Indicators

The selected indicators are well-aligned with common DeFi credit risk frameworks:
	•	Liquidation frequency directly measures protocol-detected insolvency.
	•	Collateral Ratio is a core safeguard in Compound — low ratios are prime indicators of risky behavior.
	•	Repayment coverage shows the intention and ability to meet obligations.
	•	Activity levels help identify dormant or abandoned wallets, which often carry unknown risks.

This structure provides a foundation for integrating advanced features like historical credit line usage, protocol diversification, or on-chain reputation systems.
