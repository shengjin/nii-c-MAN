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
In the new generated resulsts directory, there are a few chain*.dat file that are the Markov Chains with different beta values in the parallel tempering framework (the Algorithm are described briefly in the page). 
Use the chain that corresponds to the beta = 1.0 to generate a corner plot, in the default case is the Chain3.dat.


