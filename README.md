# Angular Super-Resolution Using ISTA

## Introduction

This project compares conventional FFT beamforming with ISTA for direction-of-arrival (DOA) estimation using a uniform linear array (ULA). Angular resolution and estimation accuracy are evaluated as functions of SNR, number of snapshots, target separation, and source coherence.

Instead of simulating the full FMCW radar chain, we use a narrowband random complex-envelope model, assuming far-field targets in the same range–Doppler cell.

## Simulation Parameters

| Parameter | Symbol | Default value |
|---|---|---|
| Speed of light | $c$ | $3\times10^8$ m/s |
| Carrier frequency | $f_c$ | 77 GHz |
| Wavelength | $\lambda$ | $c/f_c$ |
| Number of virtual antenna elements | $M$ | 8 |
| Element spacing | $d$ | $\lambda/2$ |
| Number of targets | $K$ | 2 |
| Target angles | $\theta_k$ | $-2^\circ, 2^\circ$ |
| Number of snapshots | $L$ | 32 |
| Power per source | $p_k$ | 1 |
| SNR per source | $\mathrm{SNR}_{\mathrm{dB}}$ | 10 dB |
| Angular search interval | | $[-60^\circ,60^\circ]$ |
| Angular grid spacing | $\Delta\theta$ | $0.1^\circ$ |

The ULA elements are located at $x_m=md$, for $m=0,\ldots,M-1$. Angles are measured relative to array broadside. All default values are configurable.
