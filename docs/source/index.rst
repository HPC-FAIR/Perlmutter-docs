Welcome to Perlmutter docs!
===================================

Link to official documentation: https://docs.nersc.gov/ 










Contents
--------

ssh <username>@perlmutter.nersc.gov 

**Creating virtual environment**


**Using NERSC PyTorch modules**

module load pytorch/2.0.1


hugging face-cli login
hugging face-cli whoami

export HF_DATASETS_CACHE="/pscratch/sd/s/sharma21/“

**Test starcoder**
python main.py \
    --model hf \
    --model_args pretrained=bigcode/starcoder \
    --tasks sciq \
    --device cuda:0 \
    --batch_size 8

**Add our dataset**
python main.py \
    --model hf \
    --model_args pretrained=bigcode/starcoder \
    --tasks sciq \
    --device cuda:0 \
    --batch_size 8


