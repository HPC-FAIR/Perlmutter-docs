Welcome to Perlmutter Docs!
===========================

This page documents instructions to get started with Perlmutter and issues encountered while using it. The official documentation for Perlmutter can be found `here <https://docs.nersc.gov/>`_.

Contents
--------

- `SSH into Perlmutter`_
- `Creating and Activating a Virtual Environment`_
- `Using NERSC PyTorch Modules`_
- `Huggingface Cache and Credentials`_
- `Starting an Interactive Session on Perlmutter`_
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

   export HF_DATASETS_CACHE="<path to directory where cache should be stored>"

**Load Credentials:**

.. code-block:: bash

   huggingface-cli login
   huggingface-cli whoami

Starting an Interactive Session on Perlmutter
---------------------------------------------

Before starting an interactive session, it's essential to ensure you're using the right account for allocation. To find the account, you can use the `env | grep ACCOUNT` command, which will provide output similar to:

.. code-block:: bash

   SALLOC_ACCOUNT=m2956_g
   SBATCH_ACCOUNT=m2956_g
   SLURM_JOB_ACCOUNT=m2956_g

To initiate an interactive session, use the following `salloc` command:

.. code-block:: bash

   salloc --nodes 1 --qos interactive --time 02:00:00 --constraint gpu --gpus 4 --account=m2956_g

This command requests an interactive session with:

- 1 node
- Quality of Service set to "interactive"
- A time limit of 2 hours
- On a GPU node
- Allocating 4 GPUs
- Using the account "m2959_g"

Example Job Script
------------------

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
============================
Issue: File lock issue while loading huggingface datasets/models (Eg. SentenceTransformer)
----------------------------------------

**Description:** 
An issue arises when trying to load the SentenceTransformer model `'paraphrase-MiniLM-L6-v2'`. 
The problem seems to originate from a file locking mechanism when attempting to download model weights from the huggingface_hub. I have file lock issues on Perlmutter when my python code tries to download huggingface models/datasets. The symptom is hanging execution. To debug the issue, you have to run your job in an interative session, and use ctrl+c to stop the hangs. You will then see the execution runs some infinite looping to get file locks.

Workaround: I used is to download the models/datasets manually and set the paths in my code to load them from local paths.


**Error Traceback:**
.. code-block:: python

   File "/global/u2/s/sharma21/LM4HPC/Evaluation/open_ended_eval.py", line 118, in <module>
       accuracy, results = semantic_similarity_eval(open_ended_dataset, model_name, num_rows)
   ...
   File "/global/common/software/nersc/pm-2022q4/sw/pytorch/2.0.1/lib/python3.9/site-packages/filelock/_api.py", line 230, in acquire
       time.sleep(poll_interval)
   KeyboardInterrupt

**Potential Solution:** 
1. Ensure the path for caching models is writable and has sufficient storage space.
2. If the issue persists, consider manually downloading the model weights and loading them locally.
3. Check if any other processes are simultaneously attempting to download/access the same model, leading to file lock contention.

(Note: More detailed solutions and possible causes should be investigated.)

