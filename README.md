### Wallet Credit Scoring



This project utilizes a rule-based scoring mechanism to quantify wallet-level creditworthiness in the Compound V2 system.



#### Data Collection



* **Source:** V2 Compound user activity aggregated data, presumably from on-chain data indexed via Flipside, Dune, or custom scraping.
* Derived key transactional activity and protocol-specific measurements by wallet.





#### Feature Selection Reasoning



The following attributes were chosen based on their ability to signal wallet behavior, reliability, and protocol behavior:



* ***compound\_tx\_count***: Number of Compound interactions
* ***borrow\_count***: Number of borrow transactions
* ***repay\_count***: Repay transaction count
* ***enter\_markets\_count***: Number of markets entered (diversification)
* ***unique\_ctokens***: tokens used count
* ***error\_tx\_count***: Failed/error transactions (indicator of risk)
* ***gas\_used***: Activity level proxy
* ***first\_activity\_days\_ago***: Wallet age (protocol seniority)





#### Scoring Method



1. **Normalization**: All the features normalized using Min-Max normalization.
2. **Weighting**: Heuristic weights reflecting relative contribution to risk:
3. Positive values for good behavior (e.g., repay\_count, gas usage)
4. Negative weight for risk (e.g., error\_tx\_count)
5. **Aggregation**: Weighted sum was calculated and scaled to a range of 0–1000.
6. **Final Score**: Rounded integer between 0 and 1000 indicating wallet trustworthiness.





#### Risk Indicator Rationale



###### | Feature                  | Weight | Reason                                   |

###### |--------------------------|--------|------------------------------------------|

###### | compound\_tx\_count        | +0.20  | Experience drives activity up            |

###### | borrow\_count             | +0.20  | Reflects credit engagement               |

###### | repay\_count              | +0.15  | Good repayment history                   |

###### | enter\_markets\_count      | +0.10  | Diversity of protocols = lower risk      |

###### | unique\_ctokens           | +0.10  | Portfolio diversification                |

###### | error\_tx\_count           | -0.05  | Failed transactions = poor behavior      |

###### | gas\_used                 | +0.10  | Indicates active participation           |

###### | first\_activity\_days\_ago  | +0.10  | Older wallets = more trustworthy         |





#### Scalability



* Simple to extend to Aave, Compound V3, or other lending protocols.
* Framework can be replaced by ML/XGBoost model if labeled data is available.
* Rule-based scores ensure transparency and explainability.
