Installation
=====


Requirments
------------

Installing Nii-C is essentially a matter of compiling its C source code on a POSIX-compliant system, such as Linux or macOS.
The minimum requirement for Nii-C is an implementation of the MPI protocol, as Nii-C's parallel framework is built on MPI.
Both OpenMPI and MPICH are fully supported.


   -  **git**: for cloning the code from GitHub
   -  **make** for compling the code.
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

With compilation complete, we can now run the executable, which will be sampling a linear regression model:


.. code-block:: console

   $ bash ./mjob.sh


The sampling process will be complete in a few seconds.
In the newly generated results directory, there are several chain[0-9].dat files contain Markov-chain samples collected at the distinct β of Nii-C’s parallel-tempering MCMC framework.
Always use the chain obtained by β = 1.0 to retrieve the target distribution, as chains obtained by 0 < β < 1.0 merely serve as auxiliary chains.
In this linear regression example, which is configured by the input.ini file in the source code directory, chain3.dat (β = 1.0) contains the posterior samples of the model parameters.
The details of the Nii-C's parallel-tempering algorithm and the linear regression model used in this example can be found in the code paper [1]_.





.. [1]  Jin, S., Jiang, W., Wu, D.-H. 2024. Automatic Parallel Tempering Markov Chain Monte Carlo with Nii-C. The Astrophysical Journal Supplement Series 274, 10. doi:10.3847/1538-4365/ad6300







