# GTEF: Game-Theoretic Framework for Cybersecurity in Robotic Systems

This repository contains the **official Python implementation** of the paper:  
*"GTEF: A Unified Game-Theoretic Framework for Cybersecurity Analysis in Robotic Systems"*.

GTEF provides a production-ready, open-source platform for strategic cybersecurity decision-making in robotic systems, using game theory to model defender-attacker interactions.

---

## 🚀 Key Contributions of This Work

### 1. Problem We Solve
Robotic systems face sophisticated cyberattacks, but traditional defenses (e.g., CVSS-based patching) lack **strategic reasoning** and fail to anticipate adaptive adversaries. GTEF bridges this gap by modeling security as a game between defenders and attackers.

### 2. Core Innovations

| Component | Description |
|-----------|-------------|
| **Unified Architecture** | End-to-end pipeline from vulnerability databases (RVD) to deployable security policies. |
| **Stackelberg Equilibrium** | Novel solver avoiding bilinear constraints via enumeration & linearization. |
| **Double Oracle Algorithm** | Achieves **100–1000× speedup** for large games (e.g., 500×500 matrices). |
| **Learning Dynamics** | 5 algorithms (Fictitious Play, Exp3, UCB, Hedge, MADDPG) with convergence guarantees to correlated equilibria. |
| **Mean Field Games** | Scales to **10,000+ agents** for IoT/botnet scenarios. |
| **Mechanism Design** | VCG-based bug bounty program optimization. |

### 3. Empirical Results (156 Real CVEs from RVD)
- **Stackelberg** improves defender utility by:  
  - **24.5%** over Nash Equilibrium  
  - **42.6%** over CVSS-priority  
  - **57.0%** over random baseline  
- **Statistical significance**: p < 0.001 (paired t-test, Cohen’s d = 1.87)  
- **Robustness**: >85% of optimal performance under ±20% noise  
- **Scalability**: Handles **500×500** games; Double Oracle delivers **1125× speedup** in large instances.

---

## 📦 Python Implementation Highlights

- **Modular Architecture**: Separate modules for game formulation, solvers, learning, and deployment.
- **Scalable Solvers**: Double Oracle + support enumeration for large action spaces.
- **Learning Algorithms**: Ready-to-use implementations of no-regret and multi-agent RL.
- **Integration Ready**: Compatible with Nessus, OpenVAS, Splunk, ELK, Ansible, Puppet.
- **Test Coverage**: >95% with continuous integration.
- **Documentation**: Extensive tutorials and API reference.

---

## 🧪 Quick Start

```bash
pip install gtef
```

```python
from gtef import SecurityGame, StackelbergSolver
from gtef.datasets import load_rvd_data

# Load real vulnerability data
vulns = load_rvd_data(platform="ur5")

# Build security game
game = SecurityGame.from_vulnerabilities(vulns, budget=3)

# Compute Stackelberg equilibrium
solver = StackelbergSolver(game)
strategy = solver.solve()

print(f"Optimal defender utility: {strategy.value:.2f}")
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

## 📁 Repository Structure

```
GTEF/
├── gtef/
│   ├── core/          # Game formulation & payoff matrices
│   ├── solvers/       # Nash, Stackelberg, Double Oracle
│   ├── learning/      # FP, Exp3, UCB, Hedge, MADDPG
│   ├── mfg/           # Mean Field Games
│   └── integration/   # RVD, Nessus, OpenVAS connectors
├── tests/             # >95% coverage
├── docs/              # Full documentation
└── examples/          # Tutorials & case studies
```

---

## 🤝 Contributing

Issues and pull requests are welcome. See `CONTRIBUTING.md` for guidelines.

## 📄 License

Apache 2.0

## 📬 Contact

Atefeh Deris – [deris.atefeh@gmail.com]  
Project Link: [https://github.com/atefehderis/GTEF](https://github.com/atefehderis/GTEF)
