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


## Signal Model

The steering vector for a target at angle $\theta$ is

$$
\mathbf a(\theta)=[e^{j2\pi md\sin(\theta)/\lambda}]_{m=0}^{M-1}.
$$

The received signal at antenna $m$ and snapshot $\ell$ is

$$
y_m[\ell]=\sum_{k=1}^{K}a_m(\theta_k)s_k[\ell]+w_m[\ell].
$$

For independent targets, the random complex envelopes and additive noise are generated as

$$
s_k[\ell]\sim\mathcal{CN}(0,p_k),\qquad w_m[\ell]\sim\mathcal{CN}(0,\sigma_w^2),
$$

independently across targets and snapshots (and antennas for noise). Each source envelope is shared across all antennas. Noise is independent of the sources. For equal source powers $p$, define

$$
\sigma_w^2=p\,10^{-\mathrm{SNR}_{\mathrm{dB}}/10}.
$$

Stacking all snapshots yields

$$
\mathbf Y=\mathbf A\mathbf S+\mathbf W,
\qquad \mathbf A=[\mathbf a(\theta_1),\ldots,\mathbf a(\theta_K)].
$$

The dimensions are $\mathbf Y,\mathbf W\in\mathbb C^{M\times L}$, $\mathbf A\in\mathbb C^{M\times K}$, and $\mathbf S\in\mathbb C^{K\times L}$.

For coherent targets, use a common random envelope $z[\ell]\sim\mathcal{CN}(0,1)$ with $s_k[\ell]=\sqrt{p_k}e^{j\phi_k}z[\ell]$, where $\phi_k$ is constant across snapshots.

### simple MATLAB implantation 

```matlab
c = 3e8;
fc = 77e9;
lambda = c/fc;
M = 8; d = lambda/2;
theta = [-2, 2];
K = numel(theta);
L = 32; p = 1; SNR_dB = 10;

A = exp(1j*2*pi*d/lambda*(0:M-1).' * sind(theta));
S = sqrt(p/2)*(randn(K,L) + 1j*randn(K,L));
sigma2 = p*10^(-SNR_dB/10);
W = sqrt(sigma2/2)*(randn(M,L) + 1j*randn(M,L));
Y = A*S + W;
```
