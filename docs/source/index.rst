Welcome to Perlmutter Docs!
===========================

`Official Documentation <https://docs.nersc.gov/>`_

Contents
--------

- `SSH into Perlmutter`_
- `Creating Virtual Environment`_
- `Using NERSC PyTorch Modules`_
- `Huggingface Cache and Credentials`_
- `Issues related to perlmutter`_

SSH into Perlmutter
-------------------

To SSH into Perlmutter:

.. code-block:: bash

   ssh <username>@perlmutter.nersc.gov

Creating and Activating a Virtual Environment
----------------------------

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



Issues related to Perlmutter
--------------





