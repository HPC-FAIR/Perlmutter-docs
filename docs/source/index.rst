Welcome to Perlmutter Docs!
===========================

`Official Documentation <https://docs.nersc.gov/>`_

Contents
--------

- `SSH into Perlmutter`_
- `Creating Virtual Environment`_
- `Using NERSC PyTorch Modules`_
- `Huggingface Cache and Credentials`_

SSH into Perlmutter
-------------------

To SSH into Perlmutter:

.. code-block:: bash

   ssh <username>@perlmutter.nersc.gov

Creating Virtual Environment
----------------------------

Instructions for creating a virtual environment.

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

Test Starcoder
--------------

To test the Starcoder model, run:

.. code-block:: bash

   python main.py \
       --model hf \
       --model_args pretrained=bigcode/starcoder \
       --tasks sciq \
       --device cuda:0 \
       --batch_size 8

Add Our Dataset
---------------

To add a custom dataset:

.. code-block:: bash

   python main.py \
       --model hf \
       --model_args pretrained=bigcode/starcoder \
       --tasks sciq \
       --device cuda:0 \
       --batch_size 8

