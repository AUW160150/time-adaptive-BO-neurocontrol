# Time-Adaptive Bayesian Optimization for Neural Control
Code repository for "Time-Adaptive Bayesian Optimization for Non-Stationary Neural Control Systems" 

## Overview

This repository contains the implementation and visualization code for time-adaptive Bayesian optimization with temporal forgetting kernels, designed to handle non-stationary optimization problems in neural control systems.

## Key Features

- **Temporal Forgetting Kernels**: Implementation of exponentially decaying temporal kernels for Gaussian processes
- **Comparative Analysis**: Standard BO vs. Time-aware BO under various drift conditions
- **Sensitivity Analysis**: Performance evaluation across different forgetting factors
- **Figure Generation**: Publication-quality visualizations of system architecture and performance metrics

## Repository Structure
```
├── figures/
│   ├── figure_generation.py    # Main script for generating all figures
│   └── outputs/                 # Generated figures (.png and .pdf)
├── src/
│   ├── temporal_bo.py          # Core time-adaptive BO implementation
│   ├── kernels.py              # Temporal and spatial kernel definitions
│   └── utils.py                # Helper functions
├── notebooks/
│   └── validation_analysis.ipynb  # Computational validation experiments
├── requirements.txt
└── README.md
```

## Installation
```bash
pip install -r requirements.txt
```

### Dependencies
- numpy >= 1.20.0
- matplotlib >= 3.3.0
- botorch >= 0.6.0
- torch >= 1.10.0
- gpytorch >= 1.6.0

## Usage

### Generate Figures
```python
python figures/figure_generation.py
```

This will generate:
- `Figure1_Architecture.png/pdf`: System architecture and forgetting mechanism
- `Figure2_Regret.png/pdf`: Cumulative regret comparison
- `Figure3_Sensitivity.png/pdf`: Sensitivity analysis

### Run Time-Adaptive BO
```python
from src.temporal_bo import TimeAdaptiveBO

# Initialize optimizer
optimizer = TimeAdaptiveBO(
    dim=3,                    # Parameter dimensions
    forgetting_factor=0.02,   # ε parameter
    drift_rate=0.02          # β drift rate
)

# Run optimization
results = optimizer.optimize(n_iterations=50)
```

## Mathematical Framework

The core innovation implements spatio-temporal covariance:
```
K((x,t), (x',t')) = K_spatial(x,x') · K_temporal(t,t')
```

where the temporal kernel uses exponential forgetting:
```
K_temporal(t,t') = exp(-ε|t-t'|)
```

## Key Parameters

- **ε (epsilon)**: Forgetting factor controlling memory window
- **β (beta)**: Drift rate of the objective function
- **R = τ_adapt/τ_opt**: Critical ratio for tracking feasibility

## Validation Results

- **86% regret reduction** for moderate drift (β = 0.02)
- Robust performance for ε/β ∈ [0.5, 2.0]
- Critical threshold: R > 0.3 enables successful tracking

## Citation

If you use this code in your research, please cite:
```bibtex
@techreport{timeadaptivebo2024,
  title={Time-Adaptive Bayesian Optimization for Non-Stationary Neural Control},
  author={[Your Name]},
  institution={Matter and Energy LLC},
  year={2024}
}
```

## License

MIT License - See LICENSE file for details

## Contact

For questions or collaboration: [your-email]

## Acknowledgments

This work was developed as part of the Matter and Energy LLC Technical Capability Assessment. Mathematical derivation verification performed using Claude (Anthropic).
```

## For your paper, add:
```
Implementation details and code available at: https://github.com/[your-username]/time-adaptive-bo-neurocontrol
