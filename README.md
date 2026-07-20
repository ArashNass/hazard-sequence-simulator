# Hazard Sequence Simulator

An interactive, single-page educational tool for exploring compound earthquake, aftershock, rainfall, landslide, and recovery sequences. It compares one generated hazard catalogue under baseline conditions and under user-selected resilience measures.

The application is implemented in `index.html`. It has no build step or JavaScript package dependencies and can be opened directly in a modern browser. An internet connection is used only to load the Inter font from Google Fonts; the simulator itself runs client-side.

## What it models

- **Mainshocks:** counts follow a homogeneous Poisson process, occurrence times are uniform within the selected horizon, and magnitudes follow a bounded Gutenberg-Richter distribution.
- **Aftershocks:** counts scale with mainshock magnitude and occurrence times follow a bounded modified Omori-Utsu distribution, `K(c + t)^-p`.
- **Rainfall:** storm counts and times are generated independently of the earthquake catalogue; intensities use a bounded lognormal model.
- **Landslides:** earthquake and rainfall events are screened with a logistic trigger model using slope susceptibility, rainfall or shaking severity, and transient earthquake-induced slope weakening. Asset landslide exposure controls the disruption added after a trigger; it does not change trigger probability.
- **Disruption and recovery:** each damaging event can increase a dimensionless disruption index from 0 to 1. Between events, disruption decays exponentially. The configured nominal recovery period corresponds to approximately 95% recovery, not a half-life.
- **Interrupted recovery:** an event counts as an interruption only when disruption is already above the selected downtime threshold and the event adds more than `0.015` to the disruption index.

These are transparent screening relationships, not a calibrated physical loss model.

## Features

- Five scenario choices: earthquake-rain cascade, aftershock interruption, rain-dominated slope, resilient asset, and custom
- Simple controls plus expandable advanced parameters for catalogue generation, interaction rules, asset vulnerability, recovery, and resilience measures
- Deterministic seeded runs using the Mulberry32 pseudo-random number generator
- A canvas timeline showing event markers and baseline-versus-measures disruption trajectories
- Mouse inspection, arrow-key event navigation, and animated sequence playback
- A paired Monte Carlo comparison using the same catalogue for baseline and resilience cases; the default is 500 runs and the selectable range is 50 to 2,000
- Summary statistics for mean, median, and 90th-percentile downtime, peak disruption, landslide probability, severe-disruption probability, and recovery interruption
- CSV event export, JSON simulation-state export, and PNG timeline export
- Eleven in-browser numerical checks covering repeatability, distribution bounds, finite outputs, recovery behavior, landslide probability, interruption logic, and fixed-catalogue mitigation consistency
- Light and dark themes, ARIA labeling and live announcements, keyboard-accessible timeline inspection, and mobile control and insight drawers

## Defaults and limits

- Simulation horizon: 365 days by default; allowed range 30 to 3,650 days
- Monte Carlo catalogues: 500 by default; allowed range 50 to 2,000
- Random seed: 2026 by default
- Maximum stored events per generated catalogue: 10,000
- Validation rejects parameter combinations estimated to exceed 8,000 events so the browser remains responsive
- Timeline trajectories are sampled at 261 evenly spaced points for display

The Monte Carlo analysis is reproducible for fixed settings and seed. It is performed locally in the browser and may take longer for large run counts or event-rich scenarios.

## Implementation notes

- Mulberry32 provides deterministic pseudo-random values.
- Standard-normal samples use the Box-Muller transform.
- Poisson sampling uses a direct method for rates below 30 and an acceptance-rejection method for larger rates, with a bounded fallback.
- The severity score combines peak disruption (38%), normalized downtime (30%), seven-day event clustering (18%), and landslide count (14%).
- Resilience measures can reduce disruption increments, reduce landslide susceptibility, and shorten the nominal recovery period. Baseline and measures results use the same generated catalogue for a paired comparison.

## Usage

Open `index.html` in a modern browser, or use the deployed version at [arashnassirpour.com/hazard-sequence-simulator/](https://arashnassirpour.com/hazard-sequence-simulator/).

Choose a scenario or adjust the controls, select **Run simulation**, and inspect the timeline, event story, insights, and Monte Carlo comparison. Results can be exported after a run.

## Disclaimer

This is a screening tool for education and exploration, not a site-specific hazard, engineering, insurance, or building-code assessment. The disruption index is dimensionless. Model parameters and interaction rules are simplified assumptions and must not be interpreted as predictions of physical damage, loss, safety, or regulatory performance.

## License

Licensed under the GNU Affero General Public License v3.0 only (`AGPL-3.0-only`). See `LICENSE`.
