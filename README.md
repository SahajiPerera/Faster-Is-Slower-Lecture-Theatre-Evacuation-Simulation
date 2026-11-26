# Faster-Is-Slower: Lecture-Theatre Evacuation Simulation

A social-force simulation of 150 people leaving a tiered lecture theatre, showing when a more urgent crowd leaves later.

## Live demo and notebook

- **Live demo:** {{GitHub Pages URL, e.g. https://sahajiperera.github.io/Faster-Is-Slower-Lecture-Theatre-Evacuation-Simulation/}}
- [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/SahajiPerera/Faster-Is-Slower-Lecture-Theatre-Evacuation-Simulation/blob/main/notebook/Faster-Is-Slower-Lecture-Theatre-Evacuation-Simulation.ipynb)

![Web demo screenshot](figures/fig_webdemo2.png)

## The problem

When an alarm sounds in a lecture theatre, everyone has to reach an aisle and pass one narrow door. Two questions:

1. Does a higher desired speed make the evacuation slower, and under which conditions (door width, friction)?
2. Does a circular pillar placed just in front of the exit help?

The measured quantity is the time at which the last of 150 people crosses the exit line.

## Quick start

**Notebook.** Click the Colab badge and choose *Runtime -> Run all*. The default quick mode takes about {{minutes}} minutes; set `FULL_RUN = True` in the setup cell for the full study (about 60 minutes, 10 seeds).

**Locally.**
```
pip install -r requirements.txt
jupyter notebook notebook/Faster-Is-Slower-Lecture-Theatre-Evacuation-Simulation.ipynb
```

**Web demo.** Open the live demo, or double-click `docs/index.html`. No installation or server is needed.

## User guide

See [USER_GUIDE.md](USER_GUIDE.md): controls, presets, batch mode (Monte Carlo with a PASS/FAIL check against the notebook) and timing notes.

## Model and maths

Social force model with driving force, exponential repulsion, body compression and sliding friction, integrated with velocity Verlet and accelerated with cell lists. The equations, numerical schemes and checks are in [THEORY.md](THEORY.md).

## Verification

The code is checked against analytic results, for example the single-agent relaxation $v(t)=v_0(1-e^{-t/\tau})$ with a measured convergence order close to 2, and the wall stand-off distance $-B\ln(m v_0/(\tau A))$. It also checks conservation of people, cell lists against brute force, and confidence-interval coverage.

![Verification: error scaling](figures/fig_x2_error_scaling.png)

## Concepts applied

Particle-based simulation, numerical ODE integration (velocity Verlet, stability and time-step limits), random variate generation, Monte Carlo replication with Student-$t$ confidence intervals and paired comparisons, and performance engineering (JIT compilation, neighbour lists, parallel seeds).

## Results, limitations and future work

- Without an obstacle the evacuation is fastest near 3 m/s (64.3 ± 2.0 s) and slower at 7 m/s (81.9 ± 7.2 s) for a 1.2 m door.
- The effect needs sliding friction and disappears for doors of about 1.5 m or wider.
- A pillar of radius 0.3 m, 1.5 m in front of the exit, gave no benefit at any tested speed.

Limitations: a 2D plan view with round bodies, assumed layout and rules, parameters taken from the literature rather than fitted to a real drill, one speed for everybody. Future work: other pillar shapes, mixed speeds, calibration against a real drill, larger crowds.

## Key references

1. D. Helbing, I. Farkas, T. Vicsek, "Simulating dynamical features of escape panic," *Nature* 407, 487-490 (2000).
2. J. M. Pastor et al., "Experimental proof of faster-is-slower in systems of frictional particles flowing through constrictions," *Phys. Rev. E* 92, 062817 (2015).
3. W. C. Swope et al., *J. Chem. Phys.* 76, 637-649 (1982).

Full list in [REFERENCES.md](REFERENCES.md).

## AI assistance

AI tools were used to help build and review this project; their use was checked and is documented privately.

## Licence and data credits

Code under the MIT licence (see `LICENSE`). Model parameters follow Helbing et al. (2000). The flow comparison uses published ranges from Seyfried et al. (2010); no third-party datasets are included.
