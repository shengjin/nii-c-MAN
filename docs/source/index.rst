Welcome to Nii-C's documentation!
===================================

**Nii-C** (/ni:k/) is a high-performance, general-purpose Markov Chain Monte Carlo (MCMC) library written in modern C, designed for efficient Bayesian inference across diverse scientific domains [1].

We originally developed Nii-C to deliver a fast, efficient MCMC engine tailored for exoplanet orbital-parameter fitting—addressing the performance bottlenecks we encountered in existing MCMC package. To achieve this, we implemented an Automatica Parallel Tempering MCMC (APT-MCMC) algorithm and built the library in modern C with MPI for high-performance computing and easy dependency management [1,2].

To date, we have used Nii-C to develop several applications for parameter fitting in exoplanet science, spanning astrometric signals, radial velocity curves, transit timing variation (TTV), spectral energy distribution (SED), and thermal emission spectroscopy.
Each application achieves state-of-the-art convergence speed and computational efficiency.


Contact us
----------

If you're beginning to use Nii-C for your own application or have any questions, please feel free to contact us.



Reference 
----------

1. Jin, S., Jiang, W., Wu, D.-H. 2024. Automatic Parallel Tempering Markov Chain Monte Carlo with Nii-C. The Astrophysical Journal Supplement Series 274, 10. doi:10.3847/1538-4365/ad6300

2. Jin, S., Ding, X., Wang, S., Dong, Y., Ji, J. 2022. Nii: a Bayesian orbit retrieval code applied to differential astrometry. Monthly Notices of the Royal Astronomical Society 509, 4608. doi:10.1093/mnras/stab3317


.. raw:: html

   <style>
   /* front page: hide chapter titles
    * needed for consistent HTML-PDF-EPUB chapters
    */
   section#install,
   section#usage,
   section#algorithm,
   section#development,
   section#acknowledgments {
       display:none;
   }
   </style>

Install
------------
.. toctree::
   :caption: INSTALLATION
   :maxdepth: 1
   :hidden:

   install

Usage
------------
.. toctree::
   :caption: USAGE
   :maxdepth: 1
   :hidden:

   usage
   input_parameters


Algorithm
------------
.. toctree::
   :caption: THEORY
   :maxdepth: 1
   :hidden:

   APT-MCMC
   code_structure
   core_functions
   code_variables


Development
------------
.. toctree::
   :caption: DEVELOPMENT
   :maxdepth: 1
   :hidden:

   future_development


Acknowledgments
------------
.. toctree::
   :caption: EPILOGUE
   :maxdepth: 1
   :hidden:

   acknowledgments

