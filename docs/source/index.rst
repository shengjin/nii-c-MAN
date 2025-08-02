Welcome to Nii-C's documentation!
===================================

**Nii-C** (/ni:k/) is a high-performance, general-purpose Markov Chain Monte Carlo (MCMC) library written in modern C, designed for efficient Bayesian inference across diverse scientific domains.


To date, we have used Nii-C to develop several applications for parameter fitting in exoplanet science, spanning astrometric signals, radial velocity curves, transit timing variation (TTV), spectral energy distribution (SED), and thermal emission spectroscopy.
Each application achieves state-of-the-art convergence speed and computational efficiency.


Though Nii-C's applications are currently limited to exoplanet science, it should solve the problem of high-dimensional sampling in other research domains as well.
If you're beginning to use Nii-C for your own application or have any questions, please feel free to contact us.




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

   algorithm

   code_structure
   core_functions
   code_variables


Development
------------
.. toctree::
   :caption: DEVELOPMENT
   :maxdepth: 1
   :hidden:

   development


Acknowledgments
------------
.. toctree::
   :caption: EPILOGUE
   :maxdepth: 1
   :hidden:

   acknowledgments

