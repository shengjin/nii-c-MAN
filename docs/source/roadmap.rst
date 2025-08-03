Roadmap
======================


We originally developed Nii-C to deliver a fast, efficient MCMC engine tailored for exoplanet orbital-parameter fitting—addressing the performance bottlenecks we encountered in existing MCMC package. 
To achieve this, we implemented an Automatica Parallel Tempering MCMC (APT-MCMC) algorithm and built the library in modern C with MPI for high-performance computing and easy dependency management [1,2].

Experience applying Nii-C to various exoplanet science applications has proven Nii-C’s algorithms to be efficient and reliable.
Because Nii-C’s sampling engine is both feature-complete and benchmark-fast, active development has ceased.

The development roadmap is advancing with Nii-X, though at a slow pace.
Nii-X is investigating more possibilities with a parallel MALA (Metropolis-adjusted Langevin Algorithm) framework. 



Reference 
----------

1. Jin, S., Jiang, W., Wu, D.-H. 2024. Automatic Parallel Tempering Markov Chain Monte Carlo with Nii-C. The Astrophysical Journal Supplement Series 274, 10. doi:10.3847/1538-4365/ad6300

2. Jin, S., Ding, X., Wang, S., Dong, Y., Ji, J. 2022. Nii: a Bayesian orbit retrieval code applied to differential astrometry. Monthly Notices of the Royal Astronomical Society 509, 4608. doi:10.1093/mnras/stab3317


