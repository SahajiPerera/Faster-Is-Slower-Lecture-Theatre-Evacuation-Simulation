# User guide: Lecture-Theatre Evacuation web demo

**Open the demo:** [https://sahajiperera.github.io/Faster-Is-Slower-Lecture-Theatre-Evacuation-Simulation/](https://sahajiperera.github.io/Faster-Is-Slower-Lecture-Theatre-Evacuation-Simulation/). It runs entirely in your browser (current Chrome, Edge or Firefox). Nothing is installed or uploaded.

![Labelled screenshot of the demo](figures/demo-screenshot-labelled.png)

## Controls

| Control | What it does | Range | Default |
|---|---|---|---|
| Desired speed $v_0$ | Speed every person tries to reach (m/s) | 1 to 10 | 3 |
| Exit width | Width of the single exit (m) | 0.8 to 2.5 | 1.2 |
| Pillar | Adds a circular pillar (r = 0.3 m) 1.5 m in front of the exit | on / off | off |
| Random seed | Seed for seat offsets and body sizes | any integer | 42 |
| Playback speed | Time steps drawn per animation frame | 10 to 500 | 100 |
| Start / Pause | Runs or pauses the simulation | n/a | n/a |
| Reset | Clears the run and applies the current settings | n/a | n/a |
| Presets | Calm (3 m/s), Panic (7 m/s), Wide door (2 m) | n/a | n/a |
| Number of runs | Batch size for the Monte Carlo batch (seeds = seed, seed + 1, ...) | 2 to 30 | 10 |
| Show animation during batch | Draws each batch run as it is computed (a little slower) | on / off | off |
| Run batch | Repeats the evacuation for the chosen number of seeds | n/a | n/a |

## Reading the outputs

- **Plan view:** the room seen from above with the exit at the top. Each disc is a person, coloured by speed from 0 m/s (blue) to 3 m/s and above (orange). Dense groups of orange and slow discs at the door show a jam.
- **Live tiles:** time, people out, flow over the last 5 s (persons per second) and the evacuation time once the last person has left.
- **Chart:** people out against time. A steady slope means a steady flow; a flat part means a blockage.
- **Batch results:** the mean evacuation time with a 95% confidence interval, the individual run times, the batch time, and a PASS or FAIL check against the notebook value (shown for the 1.2 m door, no pillar, at 3 or 7 m/s).

## Try this

1. Press **Calm** and run a batch of 10. The mean settles near 64 s.
2. Press **Panic** and run a batch of 10. The faster crowd needs longer, near 82 s, although everyone is trying to move faster.
3. Set the exit width to 2.0 m and repeat step 2. The faster crowd is now quicker, so the effect disappears for a wide door.
4. Tick **Pillar in front of the exit** and compare batches with and without it. The pillar does not make the evacuation faster.

A batch at 7 m/s takes about 30 s per run (about 5 minutes for 10 runs), and longer with the animation on. Keep the tab visible, because browsers slow hidden tabs.

## Check against the notebook

With the 1.2 m door and no pillar, the demo should give **64.3 ± 5 s at 3 m/s** and **81.9 ± 10 s at 7 m/s** (averaged over at least 10 runs), matching the notebook results in Section 5.4 and the reference values in Section 6.3. The batch result card shows PASS or FAIL against these.

## Limitations

- The demo uses the same model as the notebook, but JavaScript's random generator differs from NumPy's, so a single seed does not reproduce a notebook run; only the statistics agree.
- Batches of fewer than 5 runs give wide intervals, so a PASS from a small batch is weak evidence.
- Runs at 7 m/s and above are slower to compute because the time step is halved.
- Results are not reliable for doors narrower than about 0.9 m (very long clogging) or for speeds above 10 m/s.
