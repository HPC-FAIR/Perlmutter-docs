Welcome to Perlmutter Docs!
===========================

This page documents instructions to get started with Perlmutter and issues encountered while using it. The official documentation for Perlmutter can be found `here <https://docs.nersc.gov/>`_.

Check live status: https://www.nersc.gov/live-status/motd/ 

Contents
--------

- `SSH into Perlmutter`_
- `Creating and Activating a Virtual Environment`_
- `Using NERSC PyTorch Modules`_
- `Huggingface Cache and Credentials`_
- `Starting an Interactive Session on Perlmutter`_
- `Example job script`_
- `Troubleshooting`_

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
   cs $SCRATCH                                  #to avoid file lock issue
   export OPENAI_API_KEY='YOUR KEY HERE'
   echo "OPENAI_API_KEY='YOUR KEY HERE'" >> ~/.bashrc
   source lm4hpc/bin/activate
   module load pytorch/2.0.1

Note: Jobs may explicitly request to run on up to 256 GPU nodes which have 80 GB of GPU-attached memory instead of 40 GB. To request this, use -C gpu&hbm80g in your job script.

Troubleshooting 
============================
File lock issue while loading huggingface datasets/models (Eg. SentenceTransformer)
----------------------------------------

An issue arises when trying to load the SentenceTransformer model `'paraphrase-MiniLM-L6-v2'`. I have file lock issues on Perlmutter when my python code tries to download huggingface models/datasets. The symptom is hanging execution. To debug the issue, you have to run your job in an interative session, and use ctrl+c to stop the hangs. You will then see the execution runs some infinite looping to get file locks.
.. code-block:: bash

   **Error Traceback:**
   .. code-block:: python
   Add this in error File "/global/u2/s/sharma21/LM4HPC/Evaluation/open_ended_eval.py", line 118, in <module>
       accuracy, results = semantic_similarity_eval(open_ended_dataset, model_name, num_rows)
     File "/global/u2/s/sharma21/LM4HPC/Evaluation/open_ended_eval.py", line 36, in semantic_similarity_eval
       embedder = SentenceTransformer('paraphrase-MiniLM-L6-v2')
     File "/global/homes/s/sharma21/.local/perlmutter/pytorch2.0.1/lib/python3.9/site-packages/sentence_transformers/SentenceTransformer.py", line 87, in __init__
       snapshot_download(model_name_or_path,
     File "/global/homes/s/sharma21/.local/perlmutter/pytorch2.0.1/lib/python3.9/site-packages/sentence_transformers/util.py", line 491, in snapshot_download
       path = cached_download(**cached_download_args)
     File "/global/homes/s/sharma21/.local/perlmutter/pytorch2.0.1/lib/python3.9/site-packages/huggingface_hub/utils/_validators.py", line 118, in _inner_fn
       return fn(*args, **kwargs)
     File "/global/homes/s/sharma21/.local/perlmutter/pytorch2.0.1/lib/python3.9/site-packages/huggingface_hub/file_download.py", line 770, in cached_download
       with FileLock(lock_path):
     File "/global/common/software/nersc/pm-2022q4/sw/pytorch/2.0.1/lib/python3.9/site-packages/filelock/_api.py", line 260, in __enter__
       self.acquire()
     File "/global/common/software/nersc/pm-2022q4/sw/pytorch/2.0.1/lib/python3.9/site-packages/filelock/_api.py", line 230, in acquire
       time.sleep(poll_interval)
   KeyboardInterrupt

**Solution**

https://docs.nersc.gov/performance/io/dvs/#do-not-use-file-locking 
DVS doesn't support file locking. It's turned off by default for most codes at NERSC (including HDF5). If you do need to use any kind of file locking, please use Perlmutter Scratch.
Keep your entire code and environment in $SCRATCH directory and run code from there. However, keep in mind that the file system is purged, which may result in portions of the software stack being removed unexpectedly. You can back up your code at HPSS https://docs.nersc.gov/filesystems/archive/

Accessing wrong/old OpenAI API key from .bashrc
----------------------------------------
Despite updating the `OPENAI_API_KEY` environment variable in the `.bashrc` file, an older API key was being accessed when running jobs.

**Solution**
Checked if any duplicate keys are present. 

I set the environment variable in the script in both these ways and refresh the .bashrc everytime while running the jobs. Not exactly sure where the issue arises. 

.. code-block:: bash

    export OPENAI_API_KEY='YOUR KEY HERE'
    echo "OPENAI_API_KEY='YOUR KEY HERE'" >> ~/.bashrc
    source ~/.bashrc

Interactive mode times out while loading starchat-alpha model
----------------------------------------
Unable to test code on starchat-alpha in interactive mode as it takes too long to load. 
The model should be stored in huggingface cache. 
Looking into solutions <to be updated> 

