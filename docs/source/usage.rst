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


The user's data file
--------------------

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

As mentioned earlier, the user's data file must be specified in the input.ini file.  In fact, the input.ini file does far more.  It is Nii-C's main configuration file, governing every aspect of the control variables of the APT-MCMC process.
It also sets the prior ranges of all model parameters, specifies the details of the output chains, and more.
This section will introduce all the variables in the input.ini file.


- ``N_iter``: it should be a large integer. It sets the total number of iterations of the MCMC process.

- ``N_beta``: the number of parallel Markov Chains, should be set to at least ``2``.

- ``Beta_Values``: a list of doubles that give the β values of parallel chains, spearated by commas. The number of items in the list should exactly match the number of parallel chains (``N_beta``).

- ``Tune_Ladder``: whether to tune the parallel tempering ladder (``Beta_Values``) at the beginning of the APT-MCMC process. Set to ``1`` to enable tuning; set to ``0`` to disable it.  Recommend setting this option to ``0`` because the ladder tuning module is not well tested.

- ``N_stopTuneLadder``: it specifies when the ladder-tuning phase will be terminated. It should be a integer that is less than ``N_iter``. This variable is not used if ``Tune_Ladder`` is set to ``0``.

- ``scale_tune_ladder`` and ``zero_stretch``: controlling variables used in our ladder-tuning algorithm. They are ignored when ``Tune_Ladder`` is set to ``0`` (the recommended setting).

- ``N_parm``: the number of model parameters, must equal the value of ``n`` in auto_unif_prior.py that generates user_prior.c.

- ``n_iter_a_stack``: the number of interation per stack. In Nii-C, stacks are segments of Markov chains, and the entire ``N_iter`` iterations is divided into multiple stacks.

- ``n_iter_a_batch_base`` and ``n_iter_a_batch_rand``: these two variables randomly determine the number of iterations in a batch. In Nii-C, we test the swapping criteria between parallel Markov chains at the end of each batch. Therefore each stack is subdivided into many batches.  The number of iterations in a batch is randomly determined by ``n_iter_a_batch_base`` ± a random integer ≤ ``n_iter_a_batch_rand``.

- ``N_swap``: the number of swap proposals at the end of each batch. Set it to ``1`` or any larger integer.

- ``Swapmode``: this variable determines the mode used to select parallel chains for testing the swap criterion. ``0`` means that swaps are proposed only between adjacent chains, while ``1`` means that swaps are proposed between randomly chosen chains.

- ``N_stoptune``: it specifies when the proposal tuning phase will end. In Nii-C, we tune the Gaussian proposals of every model parameter during the initial burn-in stage to achieve a good acceptance rate. Set ``N_stoptune`` to a number < ``N_iter`` to ensure the Markovian property of the MCMC process.

- ``N_begintune``: when to start tuning the Gaussian proposals of every model parameter. Normally, this value should be set to ``0``.

- ``n_iter_in_tune``: it sets the number of iterations in the tuning stage. These temporary iterations are used to find good Gaussian proposal sizes for all the model parameters. They are not part of the ``N_iter`` iterations.

- ``ar_ok_lower``, ``ar_ok_upper`` and ``ar_best``:  these three variables determine if the Gaussian proposals of a chain need to be tuned at the end of a stack.  ``ar_best`` is the ideal acceptance rate.  We will not tune the proposals of a chain if its acceptance rate in a stack is between ``ar_ok_lower`` and ``ar_ok_upper``. 

- ``ar_accept_diff``: a control variable that is used in setting the Gaussian proposals of a chain in our tuning algorithm.

- ``sigma_scale_half_ratio``: scaling factor for the trial Gaussian proposals used when tuning a chain.

- ``sigma_scale_min`` and ``sigma_scale_max``: these two variables set the minimum and maximum proposal sizes of the model parameters. The minimum relative proposal size of a model parameter equal to the ``sigma_scale_min`` multiplied by the distribution range of the parameter, while the maximum relative proposal size of a model parameter equal to the ``sigma_scale_max`` multiplied by the distribution range of the parameter.

- ``sigma_jumpin_ratio``: scaling factor for the trial Gaussian proposals when the proposal size of a parameter reaches the minimum or maximum value.

- ``i_save_begin``: iteration at which saving of the Markov chains begins.

- ``init_rand_seed``: the random seed.

- ``init_gp_ratio``: the initial Gaussian proposal sizes for each model parameter are equal to the ``init_gp_ratio`` multiplied by the distribution range of the parameter.

- ``para0_min``, ``para0_max``, ``para1_min``, ``para1_min``, ...: the distribution ranges of all model parameters are set by these variables. We should write out the minimum and maximum values of all model parameters explicitly.

- ``Fout_Len``: the maximum length of the string variables when outputting Markov Chains.

- ``FoutPre`` and ``FoutSuf``: prefix and suffix appended to the filenames when saving the Markov chains.

- ``results_dir``: the result directory used to store the Markov Chains.


- ``Data_file``:  this variable is used to specify the name of the input data file.

- ``ndim_data``:  an integer variable that denots the number of columns present in the data file.

- ``Delimiter``:  the marker that separates each column in the data file.


Advanced Topics
---------------


The above four parts are everything needed to apply Nii-C to sample the posterior distribution of a new model.
If you need to adapt the Nii-C APT-MCMC framework to other sampling workflows, you will likely have to modify the other subroutines of Nii-C.
In that case, please refer to the detailed algorithmic description and implementation notes on the :doc:`../algorithm` page.


