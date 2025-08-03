Roadmap
======================


We originally developed Nii-C to deliver a fast, efficient MCMC engine tailored for exoplanet orbital-parameter fitting—addressing the performance bottlenecks we encountered in existing MCMC package. 
To achieve this, we implemented an Automatica Parallel Tempering MCMC (APT-MCMC) algorithm and built the library in modern C with MPI for high-performance computing and easy dependency management.

Experience applying Nii-C to various exoplanet science applications has proven Nii-C’s algorithms to be efficient and reliable.
Because Nii-C’s sampling engine is both feature-complete and benchmark-fast, active development has ceased.

The development roadmap is advancing with Nii-X [1]_, though at a slow pace.
Nii-X is investigating more possibilities with a parallel MALA (Metropolis-adjusted Langevin Algorithm) framework. 




.. [1] https://github.com/shengjin/nii-x.git

