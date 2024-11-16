# PanEcho: Complete AI-enabled echocardiography interpretation with multi-task deep learning

[**Gregory Holste**](https://gholste.me), Evangelos K. Oikonomou, Zhangyang Wang, [**Rohan Khera**](https://www.cards-lab.org/rohan-khera)

[**Project page**](https://www.cards-lab.org/panecho) | **PREPRINT COMING SOON** (embargo lifted 11/16/24)
-----

<p align=center>
    <img src=content/panecho.png height=500>
</p>

PanEcho is a view-agnostic, multi-task model that performs 39 reporting tasks from echocardiogram videos. The model consists of an image encoder, temporal frame Transformer, and task-specific output heads. Since PanEcho is view-agnostic, multi-view echocardiography can be leveraged to integrate information across views. At test time, predictions are aggregated across echocardiograms acquired during the same study to generate study-level predictions for each task.

## Requirements & Setup

**Note**: This repository is intended for use on a Linux machine with access to at least one NVIDIA GPU. While PanEcho was trained using NVIDIA A100 GPUs, the codebase is expected to function with virtually any recent NVIDIA GPUs. Specifically, PanEcho was developed on a Linux machine running Ubuntu 22.04.4 LTS with 8 NVIDIA A100 GPUs. All code and the following demo was also tested on an older Linux machine running Ubuntu 20.04.6 LTS with 4 NVIDIA 3090 GPUs with identical results.

### Install Packages
On a Linux machine with [Anaconda](https://docs.anaconda.com/) or [Miniconda](https://docs.anaconda.com/miniconda/) installed, run the following at the command line. Installation of required packages may take a few minutes.
```
# Navigate to the repository
cd PanEcho/  # if you are one directory above

# Install required packages
conda env create -f content/panecho.yml  # this can take up to 5-10 minutes

# Activate environment
conda activate panecho
```

## Reproducibility

While the YNHHS cohort used to develop PanEcho is not available for sharing due to IRB restrictions, the following commands were used to produce the results in the paper:

```
# Navigate to proper directory
cd src/

# Train PanEcho with distributed training across 8 GPUs
torchrun --nproc_per_node=8 train.py --output_dir panecho --model_name frame_transformer --arch convnext_tiny --fc_dropout 0.25 --normalization imagenet --augment --reduce_lr 3 --amp

### EchoNet-Dynamic fine-tuning experiments ###
# Fine-tune PanEcho on EchoNet-Dynamic
torchrun --nproc_per_node=8 train_echonetdynamic.py --model_dir_path panecho --output_dir echonetdynamic_ft --model_name frame_transformer --arch convnext_tiny --normalization imagenet --amp

# Fine-tune a randomly initialized PanEcho architecture
torchrun --nproc_per_node=8 train_echonetdynamic.py --output_dir echonetdynamic_ft --rand_init --model_name frame_transformer --arch convnext_tiny --normalization '' --amp

# Fine-tune an ImageNet-pretrained PanEcho architecture
torchrun --nproc_per_node=8 train_echonetdynamic.py --output_dir echonetdynamic_ft --model_name frame_transformer --arch convnext_tiny --normalization imagenet --amp

# Fine-tune an EchoCLIP-pretrained PanEcho architecture
torchrun --nproc_per_node=8 train_echonetdynamic.py --output_dir echonetdynamic_ft --model_name frame_transformer --arch echo-clip --normalization echo-clip --amp

# Fine-tune a Kinetics-400-pretrained 3DResNet18
torchrun --nproc_per_node=8 train_echonetdynamic.py --output_dir echonetdynamic_ft --model_name 3dresnet18 --normalization kinetics --amp

### EchoNet-Pediatric fine-tuning experiments ###
# Simply replace "echonetdynamic" with "echonetpediatric" in the previous five commands
```

## Model usage (COMING SOON)

The model and pretrained weights will become available through [Torch Hub](https://pytorch.org/hub/) via a simple one-line import as follows:
```
import torch

model = torch.hub.load('CarDSLab/PanEcho', 'PanEcho', pretrained=True)
```

## Contact

Reach out to Greg Holste (gholste@utexas.edu) and Rohan Khera (rohan.khera@yale.edu) with any questions.
