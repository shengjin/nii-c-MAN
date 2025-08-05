Usage
======================

Introduction
------------

Although Nii-C can be used to sample any target distribution, it is typically used in the context of Bayesian inference.
Specifically, it is used to sample the posterior distribution under a given model.

To adapt Nii-C for sampling the posterior of a particular evaluation model, four components must be modified: user_prior.c, user_logll.c, input.ini, and a problem-specific data file.
Below, we address each of these four components in detail.


The Prior: user_prior.c
-----------------------

In Nii-C, we use the file user_prior.c to describe the prior distribution of all model parameters. 

The Python script "auto_unif_prior.py", located in the "auto_prior" directory, is used to automatically generate user_prior.c.
At the top of auto_unif_prior.py, set "n" to the number of model parameters and run:

.. code-block:: console

   $ python auto_unif_prior.py

The script will then output a user_prior.c sized to the exact number of model parameters.
Copy the newly generated user_prior.c file into the source directory of Nii-C, replacing the old file for the linear regression example.



.. note::
   Currently, auto_unif_prior.py produces uniform priors only. For any other prior, please edit the Parts 7 and 8 of the user_prior.c manually.



The Likelihood: user_logll.c
----------------------------







