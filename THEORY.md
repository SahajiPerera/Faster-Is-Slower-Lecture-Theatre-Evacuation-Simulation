# Theory and mathematics

## 1. Problem formulation

150 people sit in a tiered lecture theatre with ten rows, two aisles and one 1.2 m exit. When an alarm sounds, each person walks to the nearest aisle and then to the exit. The questions are:

1. Does a higher desired speed increase the evacuation time, and under which conditions (door width, friction)?
2. Does a circular pillar (radius 0.3 m, 1.5 m in front of the exit) reduce the evacuation time?

The quantity of interest is the time at which the last person crosses the exit line, averaged over random seeds and reported with a 95% confidence interval.

## 2. Assumptions

1. **Two-dimensional plan view.** The sloping floor does not change the horizontal movement much for this question, and 2D keeps the model verifiable.
2. **People are discs** with radii uniform in [0.25, 0.35] m and a common mass of 80 kg. Shape effects such as turning sideways are not studied.
3. **One constant desired speed** for everybody, so the effect of speed can be isolated.
4. **Everyone knows the layout** and heads for the nearest aisle, then the exit (no search or herding).
5. **Seat backs are walls**, so each row is a narrow lane left sideways.
6. **All seats are occupied** at the start, with a random offset of at most 0.02 m.
7. **Model parameters come from the literature** (Helbing et al., 2000) and are not fitted to a real drill, so results compare scenarios.

## 3. Model equations

Person $i$ has mass $m$, position $\mathbf{x}_i$, velocity $\mathbf{v}_i$ and radius $r_i$:

$$
m\,\frac{d\mathbf{v}_i}{dt} = \mathbf{f}_i^{\mathrm{des}} + \sum_{j\neq i}\mathbf{f}_{ij} + \sum_{w}\mathbf{f}_{iw}
$$

Driving force (relaxation towards the desired velocity):

$$
\mathbf{f}_i^{\mathrm{des}} = m\,\frac{v_0\,\mathbf{e}_i - \mathbf{v}_i}{\tau}
$$

Pair force (repulsion, body compression, sliding friction), with $g(x)=\max(0,x)$:

$$
\mathbf{f}_{ij} = \Big[A\,e^{(r_{ij}-d_{ij})/B} + k\,g(r_{ij}-d_{ij})\Big]\mathbf{n}_{ij} + \kappa\,g(r_{ij}-d_{ij})\,(\Delta\mathbf{v}_{ji}\cdot\mathbf{t}_{ij})\,\mathbf{t}_{ij}
$$

A wall segment or the pillar acts in the same way, with $d_{iw}$ the distance to the nearest point of the wall and friction opposing the person's tangential velocity:

$$
\mathbf{f}_{iw} = \Big[A\,e^{(r_i-d_{iw})/B} + k\,g(r_i-d_{iw})\Big]\mathbf{n}_{iw} - \kappa\,g(r_i-d_{iw})\,(\mathbf{v}_i\cdot\mathbf{t}_{iw})\,\mathbf{t}_{iw}
$$

Here $r_{ij}=r_i+r_j$, $d_{ij}=|\mathbf{x}_i-\mathbf{x}_j|$, $\mathbf{n}_{ij}$ points from $j$ to $i$, $\mathbf{t}_{ij}$ is the tangent and $\Delta\mathbf{v}_{ji}=\mathbf{v}_j-\mathbf{v}_i$.

**Analytic stand-off distance.** A person pushed against a wall stops where the driving force balances the repulsion:

$$
\text{gap} = -B\ln\!\Big(\frac{m\,v_0}{\tau A}\Big)
$$

| Symbol | Meaning | Units | Value / range | Source |
|---|---|---|---|---|
| $m$ | Pedestrian mass | kg | 80 | Helbing et al. (2000) |
| $r$ | Body radius | m | U(0.25, 0.35) | assumed |
| $\tau$ | Relaxation time | s | 0.5 | Helbing et al. (2000) |
| $A$ | Repulsion strength | N | 2000 | Helbing et al. (2000) |
| $B$ | Repulsion range | m | 0.08 | Helbing et al. (2000) |
| $k$ | Body compression constant | N/m | $1.2\times10^{5}$ | Helbing et al. (2000) |
| $\kappa$ | Sliding friction constant | kg/(m s) | $2.4\times10^{5}$ | Helbing et al. (2000) |
| $v_0$ | Desired speed | m/s | 1.5 to 7 (pilot to 10) | swept |
| $\Delta t$ | Time step | s | 0.001 ($v_0\le5$), 0.0005 above | verified |
| exit width | | m | 1.2 (0.9 to 2.0 tested) | assumed |
| pillar | radius, distance from exit | m | 0.3, 1.5 | assumed |

## 4. Numerical method

**Velocity Verlet.** Because the forces depend on velocity, the acceleration at $t+\Delta t$ uses a predicted velocity $\mathbf{v}^*=\mathbf{v}(t)+\mathbf{a}(t)\Delta t$:

$$
\mathbf{x}(t+\Delta t)=\mathbf{x}(t)+\mathbf{v}(t)\Delta t+\tfrac12\mathbf{a}(t)\Delta t^{2},\qquad
\mathbf{v}(t+\Delta t)=\mathbf{v}(t)+\tfrac12\big[\mathbf{a}(t)+\mathbf{a}(t+\Delta t)\big]\Delta t
$$

**Accuracy.** The order of accuracy is measured, not assumed: for a single driven person the velocity error against the exact solution $v(t)=v_0(1-e^{-t/\tau})$ falls with log-log slope 2.04.

**Time-step choice.** The elastic contact has angular frequency $\sqrt{k/m}\approx 38.7\ \mathrm{s^{-1}}$ (period about 0.16 s). A time step that is too large lets fast people pass through walls ("tunnelling"). With 6 seeds, all runs are lost at $\Delta t=0.002$ s at 7 and 10 m/s, one run is lost at $\Delta t=0.001$ s at 10 m/s, and none at 0.0005 s. So $\Delta t=0.001$ s is used up to 5 m/s and 0.0005 s above. The mean evacuation time is the same within statistical noise for the three steps.

**Neighbour search.** Pair forces are cut off at 2 m. A cell list with square cells of side 2 m checks only the 3 × 3 block around each person, which costs $O(N)$ instead of $O(N^2)$. Both searches agree to $10^{-9}$ N.

**Monte Carlo statistics.** Each seed fixes the random seat offsets and radii. For $n$ finished runs the mean and 95% interval use the Student $t$ distribution:

$$
\bar T \pm t_{0.975,\,n-1}\,\frac{s}{\sqrt{n}}
$$

Comparisons between scenarios use the same seeds (common random numbers) and the interval of the paired differences. A difference is called clear only if its interval excludes zero.

## 5. Verification

| Check | Expected | Obtained | Pass? |
|---|---|---|---|
| Newton's third law for pair forces; friction opposes sliding | $\mathbf{f}_{ij}=-\mathbf{f}_{ji}$ | holds | yes |
| Single driven person against $v_0(1-e^{-t/\tau})$ | order 2 | slope 2.04 | yes |
| Wall stand-off against $-B\ln(m v_0/\tau A)$ | within 0.1% | agrees | yes |
| Pillar stand-off against $R+r+\text{gap}$ | within $10^{-3}$ m | 0.7696 m simulated and predicted | yes |
| Conservation of people (inside + outside = 150) | at every step | held | yes |
| Cell list against brute force (forces) | below $10^{-9}$ N | agrees | yes |
| Cell list against brute force (statistics, 8 seeds) | equal within noise | 98.6 s and 98.2 s | yes |
| Confidence-interval coverage on synthetic data | 0.95 | 0.950 | yes |
| Parallel batch against serial batch | identical times | identical | yes |
| Monte Carlo standard error against $n^{-1/2}$ | slope -0.5 | slope -0.43 | yes |

![Verification plot](figures/fig_x2_error_scaling.png)

## 6. Concepts applied

| Concept | Where it is used in this project |
|---|---|
| Particle-based simulation | Newton's second law with driving, pair and wall forces; neighbour lists |
| Numerical ODE integration | Velocity Verlet, measured order of accuracy, stiffness limit on the time step |
| Random variate generation | Random seat offsets and body radii from uniform distributions |
| Monte Carlo and confidence intervals | Every result is a mean over seeds with a 95% Student-$t$ interval |
| Variance reduction (common random numbers) | Paired comparisons between speeds, door widths and pillar / no pillar |
| Verification and validation | Analytic limits, conservation checks, convergence and coverage tests, comparison with published flow ranges |
| Performance engineering | JIT-compiled kernels (43 times faster than NumPy broadcasting), cell lists, parallel seeds |

## 7. References

See [REFERENCES.md](REFERENCES.md).
