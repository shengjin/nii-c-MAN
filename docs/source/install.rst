Installation
=====


Requirments
------------

Installing Nii-C is essentially a matter of compiling its C source code on a POSIX-compliant system, such as Linux or macOS.
The minimum requirement for Nii-C is an implementation of the MPI protocol, as Nii-C's parallel framework is built on MPI.
Both OpenMPI and MPICH are fully supported.

Prerequisites for Nii-C:

   -  **git**: for cloning the code from GitHub.
   -  **make**: for compling the code.
   -  **MPI**: can be installed via OpenMPI or MPICH; either option is fine.


Installation
------------

To install Nii-C, first get the source code:

.. code-block:: console

   $ git clone https://github.com/shengjin/nii-c.git


Then, enter the source code directory:

.. code-block:: console

   $ cd nii-c/src/

Finally, compile the code:

.. code-block:: console

   $ make

With compilation complete, we can now run the executable, which will be sampling a linear regression model as an example:


.. code-block:: console

   $ bash ./mjob.sh


The sampling process will be complete in a few seconds.
In the newly generated results directory, there are several chain[0-9].dat files contain Markov-chain samples collected at the distinct β of Nii-C’s parallel-tempering MCMC framework.
In this linear regression example, which is configured by the "input.ini" file in the source code directory, chain3.dat (β = 1.0) contains the posterior samples of the model parameters.
Please see the Nii-C's code paper [1]_ for the details of the Nii-C's parallel-tempering algorithm and the linear regression model used in this example.


.. note::
    We should always use the chain obtained by β = 1.0 to retrieve the target distribution, as chains obtained by 0 < β < 1.0 merely serve as auxiliary chains.


This linear regression model is simple yet contains all four elements necessary for MCMC sampling with Nii-C: "user_prior.c" specifying the prior distribution, "user_logll.c" defining the likelihood function, "input.ini" configuring Nii-C's APT-MCMC sampling process, and "xy.dat" containing the input data for model evaluation.
We will describe them in detail on the :doc:`../usage` page.



.. [1]  https://ui.adsabs.harvard.edu/abs/2024ApJS..274...10J/abstract







