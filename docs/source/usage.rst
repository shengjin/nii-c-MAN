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

         double logll_beta(double *ptr_one_chain, int nline_data, double *data_NlineNdim, double beta_one);

Where the "\*ptr_one_chain" is a one-dimensional array of model paramters, "\*data_NlineNdim" is a one-dimensional array of flattened input datafile, "nline_data" is the line number of the user's input datafile, "beta_one" is the beta value of each parallel tempering chain.

Then, within the main body of logll_beta, we can unpacked from the ptr_one_chain array all the model parameters as follows:

.. code-block:: C

    double para0;
    double para1;
    double para2;
    double para3;
    para0 = *ptr_one_chain;
    para1 = *(ptr_one_chain+1);
    para2 = *(ptr_one_chain+2);
    para3 = *(ptr_one_chain+3);

The remaining components of the likelihood function are model-specific and depend on the details of the user's input data file.
That's the main task of applying Nii-C to a user's model, and the user should work carefully on it.
In the "model" directory of the source code, there are several user_logll.c files for different applications that can be used as exmaples.


.. note::
   To use the Nii-C's APT-MCMC algorithm, remember to apply the final tempering step with beta_one at the end of the likelihood function. Depending on how the likelihood is expressed in your implementation, this could be something like "logll*beta_one" or "pow(likelihood, beta_one)."


The input data file
------------------

By default, Nii-C expects the user to supply a data file for model evaluation.
The user must specify the name of the data file in the "input.ini" file, as well as the number of columns in the data file and the delimiter for each column.
Within the "input.ini" file, there are three variables associated with the user's datafile, as follows:

.. code-block:: Markdown

    Data_file:  this variable is used to specify the name of the input data file.
    ndim_data:  an integer variable that denots the number of columns present in the data file.
    Delimiter:  the marker that separates each column in the data file.

The user's data file will be loaded into memory and passed to the logll_beta function via the one-dimensional array data_NlineNdim.

.. note::
   No matter how many columns the original data file has, Nii-C will load the multi-column data file and flatten it into a one-dimensional array called data_NlineNdim. Therefore, the user must transform the one-dimensional array back into its original multi-column form when calculating the likelihood function. Converting the one-dimensional array back to its original multi-column form can be something like data_NlineNdim[i_line*ndim_data+j_column].


The input.ini file
------------------

The input.ini file sets the controlling variables of the Nii-C's APT-MCMC process, ...


