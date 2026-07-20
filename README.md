# 5G NR System-Level Simulator (MATLAB)

A single-file, dependency-light **5G NR system-level simulator** written in
MATLAB. It models a multi-cell hexagonal network with mobile users, a 3GPP
TR 38.901 UMa channel with CDL-E fast fading and shadow fading, massive
MIMO with regularized zero-forcing precoding, real MCS/CQI link adaptation
(3GPP TS 38.214 table), a Proportional Fair scheduler with QoS/slicing
awareness, HARQ with incremental redundancy combining, EN-DC (NSA) dual
connectivity, and A3-event handover with hysteresis, time-to-trigger, and
X2 delay. It runs tick-by-tick, renders a live dashboard while simulating,
and produces a full set of KPI plots and a console report at the end.

## Features

- **Network topology** — hexagonal multi-cell layout, 3-sector cells, random walk mobility with boundary bouncing
- **Traffic models** — eMBB, URLLC, mMTC with ON/OFF burstiness
- **Channel model** — 3GPP UMa path loss, log-normal shadow fading, CDL-E-style fast fading with Doppler
- **Massive MIMO** — 64x4 antenna configuration, regularized ZF precoding, MU-MIMO grouping
- **Link adaptation** — 3GPP TS 38.214 MCS table (29 entries), CQI mapping, transport block size calculation
- **Scheduling** — Proportional Fair scheduler with slice-priority and buffer-aware weighting
- **HARQ** — up to 4 retransmissions per process, incremental redundancy (IR) combining, BLER model
- **QoS / network slicing** — per-slice guaranteed bit rate (GBR) enforcement and latency-based drops
- **Dual connectivity** — EN-DC (NSA) with an LTE anchor carrier
- **Handover** — A3 event, hysteresis, time-to-trigger (TTT), and X2 signalling delay
- **Live visualization** — real-time 5-panel dashboard (topology, throughput, SINR, latency, handovers) while the simulation runs
- **Post-run analysis** — 9-panel performance figure, CDF curves, HARQ breakdown, KPI dashboard, MU-MIMO/scheduler figure, and a console summary report

## Repository structure

```
.
├── main.m          # entire simulator: config, network init, and all pipeline stages
├── figures/         # (add your exported .png/.fig outputs here)
├── LICENSE
└── README.md
```

> The simulator is intentionally kept as a single script with local
> functions (MATLAB supports local functions in scripts since R2016b), so
> it can be run by simply opening `main.m` and pressing Run — no path
> setup or package installation required.

## Requirements

- MATLAB R2016b or later (for local functions in scripts)
- **Statistics and Machine Learning Toolbox** (uses `poissrnd`, `exprnd`, `randsample`, `prctile`, `ecdf`)
- No Communications Toolbox dependency — the channel model, MIMO precoding, and link adaptation are implemented directly

## Usage

```matlab
main
```

Running the script will:
1. Load configuration (`config_complete`) — 7 cells, 10 users/cell, 1000 ms simulation, 100 MHz @ 3.5 GHz, 64x4 MIMO
2. Initialize the network and users
3. Run the tick-by-tick simulation loop, updating a live dashboard (`Figure 1`)
4. Print progress every 5% and a final KPI report to the console
5. Generate five additional figures: Performance Analysis, CDF Curves, HARQ Analysis, KPI Dashboard, and MU-MIMO & Scheduler

### Adjusting the scenario

All scenario parameters live in `config_complete()` at the top of `main.m`:

| Parameter | Meaning | Default |
|---|---|---|
| `cfg.numCells` | number of hexagonal cells | 7 |
| `cfg.usersPerCell` | users per cell | 10 |
| `cfg.simTime` | simulation duration (ticks/ms) | 1000 |
| `cfg.bandwidth_MHz` / `cfg.fc_GHz` | carrier bandwidth / frequency | 100 MHz / 3.5 GHz |
| `cfg.N_bs_ant` / `cfg.N_ue_ant` | BS / UE antenna count | 64 / 4 |
| `cfg.schedulerType` | scheduler | `'PF'` |
| `cfg.enableDC` | enable EN-DC dual connectivity | 1 |

## Output KPIs

The console report and figures cover: DL/UL throughput (mean, median, 5th/95th percentile), SINR distribution, latency, handover counts, HARQ ACK/NACK/drop breakdown, Jain's fairness index, dual-connectivity active fraction, and per-slice (eMBB/URLLC/mMTC) performance.

## Adding figures to this repo

Export the six generated figures (e.g. `saveas(gcf, 'figures/kpi_dashboard.png')` after each `figure(...)` block, or use `exportgraphics`) and drop them into `figures/` before pushing, then reference them here, e.g.:

```markdown
![KPI Dashboard](figures/kpi_dashboard.png)
```

## License

Released under the MIT License — see [LICENSE](LICENSE).
