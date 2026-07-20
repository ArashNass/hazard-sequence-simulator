# Hazard Sequence Simulator

An interactive, single-page educational tool that models compound natural hazards — earthquakes, aftershocks, rainfall, and landslides — and shows how they interact and interrupt the recovery of an asset over time.

The whole app lives in `index.html`: no build step, no dependencies, just open it in a browser.

## What it does

The simulator generates a deterministic, seeded sequence of hazard events and tracks a **disruption index** (0–100%) representing operational impairment of an asset over a simulated period (365+ days):

- **Mainshocks** — homogeneous Poisson process with a Gutenberg-Richter magnitude distribution
- **Aftershocks** — modified Omori-Utsu temporal clustering (`K(c+t)^-p`)
- **Rainfall** — generated independently of seismic events
- **Landslides** — triggered via logistic screening combining slope susceptibility, rainfall intensity, and seismic-induced slope weakening

Recovery follows an exponential curve with a user-defined half-life, and repeated hazard hits amplify damage if the asset hasn't fully recovered — modeling realistic compounding risk rather than treating each hazard independently.

## Features

- **Scenario presets**: earthquake-rain cascade, aftershock interruption, rain-dominated slope, resilient asset, and a fully custom mode
- **Advanced parameters** (hidden by default): Omori law coefficients, rainfall log-dispersion, damage interaction multipliers, recovery acceleration
- **Interactive timeline**: canvas-rendered event markers and disruption trajectories (baseline vs. mitigated), with playback scrubbing and keyboard navigation
- **Monte Carlo analysis**: 500 paired catalogues comparing downtime distributions with and without mitigation
- **Comparison table**: mean downtime, 90th percentile downtime, landslide probability, and other summary metrics
- **Data export**: CSV (event-by-event timeline), JSON (full simulation state), and PNG (high-res timeline chart)
- **Built-in self-tests**: 13 in-browser checks validating RNG repeatability, bounds enforcement, recovery monotonicity, and mitigation consistency
- **Accessibility**: ARIA labels, live regions, and screen-reader-only content
- **Responsive design**: light/dark themes, mobile layout with collapsible drawers for controls and insights

## Implementation notes

- Deterministic **Mulberry32** PRNG for reproducible runs
- Normal sampling via Box-Muller transform, Poisson sampling with lambda-dependent algorithm selection, log-gamma for statistical computations
- Severity index combines peak disruption (38%), downtime (30%), event clustering (18%), and landslide count (14%)
- Validation guards against excessive computation (flags scenarios expected to exceed 8,000 events)

## Usage

Just open `index.html` in a browser — no installation or server required.

## Disclaimer

This is a **screening tool for education and exploration**, not a site-specific hazard model. The disruption index is a dimensionless, simplified measure and should not be interpreted as physical damage, insurance loss, or a building-code performance metric. The underlying statistical models (Omori-Utsu, Gutenberg-Richter) follow standard USGS references, but interaction assumptions (e.g. aftershock damage coefficients, disruption thresholds) are explicit simplifications, not physical laws.
