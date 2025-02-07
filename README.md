# Dynamic-to-static Image Transformation
> Dataset and code for our Pattern-Recognition paper: 《[A Coarse-to-Fine Approach for Dynamic-to-static Image Transformation](https://doi.org/10.1016/j.patcog.2021.108373)》

<img src=".\examples\example.png" width="800px" />

## News


## Installation

- Install Torch (tested on 1.2.0)

## Dataset

- Synthetic Dataset: [EmptyCities](https://github.com/BertaBescos/EmptyCities_SLAM) (for train, test and validation)

  >this dataset is generated with [CARLA 0.8.2](https://drive.google.com/file/d/1ZtVt1AqdyGxgyTm69nzuwrOYoPUn_Dsm/view) by Berta et al. in [IEEE TRO2020 Paper](https://arxiv.org/abs/2010.07646);
  >
  >and it is available in [this link](https://drive.google.com/drive/folders/1aDO7_HtVkCncGew9ZMpDJ9KCT4fYD8hm?usp=sharing);
  >
  >how to generate dataset by CARLA could be found [here](https://github.com/bertabescos/EmptyCities);

- Synthetic Dataset: New (**with large dynamic  rate range!!**)

  > this dataset is generated with [CARLA 0.8.2](https://drive.google.com/file/d/1ZtVt1AqdyGxgyTm69nzuwrOYoPUn_Dsm/view) by us for further evaluation;
  > and it is available in [this link]();

## Train

- First step: train coarse network

```
python train.py --gpu_ids 0 --batchSize 4 --netG unet_256 --netD basic 
--mode Coarse --name CoarseNet_unet8_load400
```

- Second step: end-to-end train coarse-to-fine network

```
python train.py --gpu_ids 0 --batchSize 4 --netG Coarse2fineNet --netD SA 
--mode Coarse2fine --name Coarse2fineNet_unet8_1206
```

- Visualization on TensorBoard for training is supported.

```
tensorboard --logdir model_logs --port 6006
```

## Test

- test coarse network

```
python test.py --phase test --gpu_ids 0 --eval --no_flip --netG unet_256 
--mode Coarse --name CoarseNet_unet8_load400 --which_epoch 21
```

- test coarse2fine network

```
python test.py --phase test --gpu_ids 0 --eval --no_flip --netG Coarse2fineNet 
--mode Coarse2fine --name Coarse2fineNet_unet8_1206 --which_epoch 42
```

## Transfer to Real Data

- load pretrained model (i.e. which epoch) on CARLA synthetic dataset, then continue train from next one epoch by an appropriate learning rate on [Cityscapes Dataset](https://www.cityscapes-dataset.com/).

```
python train.py --gpu_ids 0 --batchSize 1 --lr 0.0001 --netG Coarse2fineNet --netD SA 
--mode Transfer --name transferModel_0614 --continue_train --which_epoch 42 --epoch_count 43
```

- test

```
python test.py --phase val --gpu_ids 0 --eval --no_flip --netG Coarse2fineNet 
--mode  Transfer --name transferModel_0614 --which_epoch 42
```

## Resource

[Related Resource in my BaiduCloud](https://pan.baidu.com/s/1KpuWKwNpkP3xizcLg5k-ww)(Extraction-code:5250)

## Citation
```
@article{WANG2022108373,
title = {A coarse-to-fine approach for dynamic-to-static image translation},
journal = {Pattern Recognition},
volume = {123},
pages = {108373},
year = {2022},
issn = {0031-3203},
doi = {https://doi.org/10.1016/j.patcog.2021.108373},
url = {https://www.sciencedirect.com/science/article/pii/S0031320321005537},
author = {Teng Wang and Lin Wu and Changyin Sun},
```
.

## Acknowledge

- Postgraduate Research＆Practice Innovation Program of Jiangsu Province (SJCX20_0035)
- [Github: Rethinking-Inpainting-MEDFE](https://github.com/KumapowerLIU/Rethinking-Inpainting-MEDFE)

- [Github: pytorch-CycleGAN-and-pix2pix](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix)
