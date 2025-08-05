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

In Nii-C, we use the user_logll.c file to describe a model's likelihood function.
Because the likelihood function is inherently model-specific, it cannot be auto-generated in the same way as the user_prior.c file.

To facilitate applying Nii-C to different models, we designed a template function that wraps the coding of the user-specific likelihood function.
The template likelihood function is called:

.. code-block:: C
         double logll_beta(double \*ptr_one_chain, int nline_data, double \*data_NlineNdim, double beta_one)

Where the \*ptr_one_chain is one-dimensional array of the model paramters, \*data_NlineNdim is a one-dimensional array of the flatted user's datafile, nline_data is the line number of the user's datafile, beta_one is the beta value of each parallel tempering chain.

Where 


However, we

.. code-block:: C

    double para0;
    double para1;
    double para2;
    double para3;
    para0 = *ptr_one_chain;
    para1 = *(ptr_one_chain+1);
    para2 = *(ptr_one_chain+2);
    para3 = *(ptr_one_chain+3);


.. note::
   Don't forget the tempering at the end of the likelihood function. Otherwise .. not be used.



