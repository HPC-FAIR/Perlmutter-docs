Welcome to Perlmutter Docs!
===========================

`Official Documentation <https://docs.nersc.gov/>`_

Contents
--------

- `SSH into Perlmutter`_
- `Creating and Activating a Virtual Environment`_
- `Using NERSC PyTorch Modules`_
- `Huggingface Cache and Credentials`_
- `Example job script`_
- `Issues related to Perlmutter`_

SSH into Perlmutter
-------------------

To SSH into Perlmutter:

.. code-block:: bash

   ssh <username>@perlmutter.nersc.gov

Creating and Activating a Virtual Environment
--------------------------------------------

.. code-block:: bash

   python -m venv /path/to/new/virtual/environment
   source /path/to/virtual/environment/bin/activate

Using NERSC PyTorch Modules
---------------------------

To load the PyTorch module, use the following command:

.. code-block:: bash

   module load pytorch/2.0.1

Huggingface Cache and Credentials
---------------------------------

**Set Huggingface Cache:**

.. code-block:: bash

   export HF_DATASETS_CACHE="/pscratch/sd/s/sharma21/"

**Load Credentials:**

.. code-block:: bash

   huggingface-cli login
   huggingface-cli whoami

Example Job Script
-----------------

Here is an example of a job script:

.. code-block:: bash

   #!/bin/bash
   #SBATCH -A m2956
   #SBATCH -C gpu
   #SBATCH -q regular
   #SBATCH -t 3:00:00
   #SBATCH -N 1
   #SBATCH -c 32

   export HF_HOME=/pscratch/sd/s/sharma21/hf/
   cd /global/homes/s/sharma21/lm-evaluation-harness
   source lm-eval/bin/activate
   cd /global/homes/s/sharma21/bigcode-evaluation-harness
   module load pytorch/2.0.1

Note: Jobs may explicitly request to run on up to 256 GPU nodes which have 80 GB of GPU-attached memory instead of 40 GB. To request this, use -C gpu&hbm80g in your job script.

Issues related to Perlmutter
----------------------------

(Your text here)
