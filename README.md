# Phishing Website Detection Using Machine Learning and Adversarial Resilience

A machine learning research project focused on detecting phishing URLs and evaluating model robustness against adversarial evasion techniques. Completed as part of my M.S. Thesis in Cybersecurity at the Grove School of Engineering, CCNY (CUNY).

> 📄 This repository is a curated showcase of the project's approach and results. Full implementation details and thesis document available upon request.

---

## Problem

Phishing attacks remain a major cybersecurity threat, using deceptive URLs to impersonate legitimate websites. Traditional blacklist and rule-based detection systems are easily bypassed through minor lexical changes — such as homoglyph substitutions or typographical variations — that evade static pattern matching.

**Goal:** Build ML classifiers for phishing URL detection, then rigorously evaluate their robustness against adversarially manipulated URLs, and identify defenses that improve resilience.

---

## Data Sources

- **PhishTank** — community-verified phishing URL database
- **OpenPhish** — real-time phishing feed
- **Cisco Umbrella** — top-ranked legitimate domain list (benign class baseline)

*(Exact sampling, cleaning, and feature extraction pipeline not included in this public repo.)*

---

## Approach (High-Level)

```
Raw URLs → Feature Extraction (lexical + character-level) → Model Training →
Adversarial URL Generation → Robustness Evaluation → Defense Mechanisms → 
Cross-Dataset & Domain-Mix Evaluation
```

**Models evaluated:**
- Logistic Regression
- Random Forest
- XGBoost

**Adversarial evaluation:** Adversarial URLs were generated using techniques that mimic real-world evasion strategies, including character-substitution and typosquatting-style manipulations, to test how well each model held up outside clean training conditions.

**Defenses explored:** Two resilience-improving techniques were implemented and evaluated to help models recover performance lost under adversarial conditions.

*(Specific feature engineering methods, adversarial generation logic, and defense implementation are intentionally omitted here — see full thesis for methodology.)*

---

## Results

### Clean-Data Performance

All three models achieved strong performance on unperturbed test data:

| Metric | Result |
|--------|--------|
| AUC    | > 0.99 |
| F1-Score | ≈ 0.98 – 0.99 |

### Adversarial Robustness

- Homoglyph-based attacks caused the greatest performance degradation across models
- **XGBoost** was most sensitive, showing up to a **5.7% drop in F1** under homoglyph attacks — tree-based models rely heavily on n-gram patterns, which are disrupted by Unicode confusable characters
- Defense mechanisms (adversarial training + character normalization) improved resilience across models

### Cross-Dataset Generalization
*(Trained on PhishTank, tested on OpenPhish)*

- **Logistic Regression** generalized strongly: F1 ≈ 0.98 – 1.00
- **XGBoost** showed moderate sensitivity to distribution shift

### Domain-Mix Training
*(80% PhishTank + 20% OpenPhish)*

- Improved adversarial robustness across models
- **XGBoost F1 improved by up to 7.9 percentage points** compared to single-domain training

**Clean vs. Adversarial Performance**
![Baseline Clean Vs Adversarial F1](results/Baseline%20Clean%20Vs%20Adverserial%20F1.png)
*Comparison of F1-scores across models on clean vs. adversarially perturbed test data.*

**Robustness Gap (PhishTank)**
![Robustness Gap Phish Tank](results/Robustness%20Gap%20%28Phish%20Tank%29.png)
*Performance drop-off observed per model when moving from clean to adversarial conditions on the PhishTank dataset.*

**Change in Robustness Gap**
![Change in Robustness Gap](results/Change%20in%20Robustness%20Gap.png)
*Shift in robustness gap after applying defense mechanisms (adversarial training + character normalization).*

**Robustness Gap Across Attack Types (Domain-Mix Training)**
![Robustness Gap Across Attacks Domain Mix Module](results/Robustness%20Gap%20Across%20Attacks%28Domain%20Mix%20Module%29.png)
*Robustness gap broken down by attack type after domain-mix training.*

**Robustness Gap After Domain-Mix Adaptation**
![Robustness Gap after Domain Mix Adaptation](results/Robustness%20Gap%20after%20DOmain%20Mix%20Adaptation.png)
*Overall robustness improvement after domain-mix training (80% PhishTank + 20% OpenPhish) — XGBoost F1 improved by up to 7.9 percentage points.*

**Subdomain Obfuscation Attack Performance**
![Subdomain Obfuscation](results/Subdomain%20Obfuscation.png)
*Model performance under subdomain obfuscation-style adversarial attacks.*

---

## Key Takeaways

- Strong clean-data performance does not guarantee adversarial robustness — evaluation under attack conditions is essential
- Homoglyph attacks are a particularly effective evasion strategy against tree-based lexical classifiers
- Diverse, domain-mixed training data measurably improves robustness against distribution shift and adversarial manipulation
- Logistic Regression showed the strongest generalization across datasets, while ensemble methods required additional defenses to match that resilience

---

## Future Work

- Expanding the adversarial space to more sophisticated URL manipulation techniques
- Integrating deep learning architectures for richer feature representation
- Real-world deployment evaluation (e.g., browser filters, email gateways) to assess operational robustness

---

## Tech Stack

`Python` · `scikit-learn` · `XGBoost` · `pandas` · `NumPy` · `Matplotlib`

---

## Repository Scope

This repository contains a high-level overview, architecture, and results of the research. Core feature engineering logic, adversarial URL generation methods, and defense implementation details are not published here.

📄 Full thesis paper and detailed methodology available upon request — feel free to reach out.

---

## License

All rights reserved. This repository is shared for portfolio and demonstration purposes only. No part of this content may be reproduced or reused without explicit permission.
