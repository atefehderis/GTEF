---

```markdown
# GTEF: Reproducible Security-Game Patch Prioritization for Robotic Cyber-Physical Systems

This repository contains the official Python implementation of the paper:  
**"GTEF: A Unified Game-Theoretic Framework for Cybersecurity Analysis in Robotic Systems"** (arXiv 2024).

GTEF provides an **auditable, reproducible pipeline** for strategic patch prioritization in Internet-connected robotic CPS and Industrial IoT (IIoT) environments. It converts public vulnerability records (CVE, RVD, vendor advisories) into security games, computes equilibrium strategies, and evaluates worst-case expected loss under adversarial best responses.

> **Note:** This paper does **not** propose a new equilibrium algorithm. Its contribution is an integrated, reproducible application of established game-theoretic methods to CPS vulnerability data.

---

## 🎯 Key Features

| Component | Description |
|-----------|-------------|
| **Payoff Construction** | Separates CVSS impact from scenario reachability (avoids double-counting) |
| **Attack Graph Model** | Non-additive precondition-based model over zone (edge/cell/control) & privilege (none/user/root) |
| **Maximin (Security) Strategy** | Single LP formulation for zero-sum projection |
| **Strong Stackelberg Equilibrium** | Multiple-LP method (Conitzer & Sandholm 2006) with incentive-compatibility constraints |
| **Double Oracle Solver** | Exact for zero-sum games; terminates in finite iterations; exactness gap < 1e-12 |
| **Learning Diagnostics** | Fictitious Play (Nash gap), Hedge & Exp3 (time-averaged regret → CCE) |
| **Adversarial Tie-Breaking** | Conservative evaluation: attacker chooses worst defender payoff among tied best responses |

---

## 📊 Empirical Results (from the paper)

### Synthetic Benchmark (100 instances, 8 vulns, budget 2)

| Strategy | Expected Loss (↓ better) | vs. CVSS-priority |
|----------|--------------------------|-------------------|
| CVSS-priority | 105.5 | — |
| Greedy | 101.7 | — |
| Best Pure | 94.3 | −11% |
| **Maximin** | **60.7** | **−42%** |
| **Stackelberg** | **63.0** | **−40%** |

### Universal Robots Scenario (11 public records, budget 3, 232×139 game)

| Strategy | Expected Loss | Reduction vs. CVSS-priority |
|----------|---------------|----------------------------|
| CVSS-priority | 77.9 | — |
| CVSS-Softmax | 68.8 | −12% |
| **Maximin** | **44.2** | **−43%** |
| **Stackelberg** | **45.3** | **−42%** |

> Game-theoretic strategies reduce worst-case loss by **34–57%** relative to non-game-theoretic baselines.

### Solver Scalability

- **Dense games:** LP (HiGHS) faster than Double Oracle at all sizes (1.3s vs 40.8s at 500×500)
- **Low-rank games (rank 6):** Double Oracle achieves up to **7.6× speedup** at 500×500 with support size 8–9

---

## 🧪 Quick Start

```bash
pip install gtef
```

```python
from gtef import SecurityGame, MaximinSolver
from gtef.datasets import load_rvd_data

# Load real vulnerability data (Universal Robots scenario)
vulns = load_rvd_data(platform="ur5")

# Build security game with budget constraint
game = SecurityGame.from_vulnerabilities(vulns, budget=3)

# Compute maximin strategy (security strategy via linear programming)
solver = MaximinSolver(game)
strategy = solver.solve()

print(f"Maximin defender utility: {strategy.value:.2f}")
```

---

## 📁 Repository Structure

```text
GTEF/
├── gtef/
│   ├── core/              # Game formulation, payoff matrices, attack graph
│   ├── solvers/           # Maximin (LP), Stackelberg (multi-LP), Double Oracle
│   ├── learning/          # Fictitious Play, Hedge, Exp3
│   └── integration/       # RVD, CVE, NVD connectors
├── tests/                 # >95% coverage
├── docs/                  # Full documentation
├── examples/              # Tutorials & case studies
│   ├── synthetic_benchmark/
│   └── ur_scenario/
└── data/                  # Data manifest & fixed seeds (reproducibility)
```

---

## 🔬 Reproducibility

The artifact includes:

- **Fixed random seed** for all stochastic components
- **Data manifest** listing all public records used (CVE, RVD, vendor advisories)
- **Environment specification** (conda/pip)
- **Scripts** to regenerate every table and figure from the paper

All results can be reproduced by running:

```bash
python scripts/run_all_experiments.py
```

---

## 📚 Citation

If you use GTEF in your research, please cite:

```bibtex
@article{deris2024gtef,
  title={GTEF: A Unified Game-Theoretic Framework for Cybersecurity Analysis in Robotic Systems},
  author={Deris, Atefeh and Navidi, Hamidreza and Keshavarzi, Behbod},
  journal={arXiv preprint arXiv:xxxx.xxxxx},
  year={2024}
}
```

---

## 🤝 Contributing

Issues and pull requests are welcome. Please see `CONTRIBUTING.md` for guidelines.

---

## 📄 License

Apache 2.0

---

## 📬 Contact

Atefeh Deris – [deris.atefeh@gmail.com](mailto:deris.atefeh@gmail.com)  
Project Link: [https://github.com/atefehderis/GTEF](https://github.com/atefehderis/GTEF)
```

---
