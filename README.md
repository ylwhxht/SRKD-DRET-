
# Sunshine to Rainstorm: Cross-Weather Knowledge Distillation for Robust 3D Object Detection (AAAI2024)


This is a official code release of SRKD&DRET (Sunshine to Rainstorm: Cross-Weather Knowledge Distillation for Robust 3D Object Detection). 

## Framework
![image](https://github.com/ylwhxht/SRKD-DRET/blob/main/framework.png?raw=true)

## Getting Started (Installation, Environment)

This code is mainly based on [OpenPCDet](https://github.com/open-mmlab/OpenPCDet), [Fog Simulation work](https://github.com/MartinHahner/LiDAR_fog_sim) and [SPRAY](https://github.com/Wachemanston/Reconstruction-and-Synthesis-of-Lidar-Point-Clouds-of-Spray).

Configuration files and installation can refer to their official website guidelines.

We tested the K-Radar detection frameworks on the following environment:

* Python 3.8.13 (3.10+ does not support open3d.)
* Ubuntu 18.04/20.04
* Torch 1.11.0+cu113
* CUDA 11.3

More detailed guidance coming soon...

## DRET

### config

**please just refer to [SPRAY](https://github.com/Wachemanston/Reconstruction-and-Synthesis-of-Lidar-Point-Clouds-of-Spray), or we will release more detailed guidance later.**

*note:We have only uploaded some of the code that we have modified. If there are too many other libraries and files, please follow the SPRAY official website's instructions for installation. If you have any questions, please feel free to raise them*

The workload of reproducing SPRAY based on Unity3D virtual engine is relatively high.

As a consequence, we can provide the rain particles obtained during the DRET process (txt file, Nx3 (x, y, z)) If needed, please contact huangxun@stu.xmu.edu. But please note that we cannot provide the final rainy point cloud due to Waymo's policy. Just download the rain particles and run the second stage of scene processing code to obtain the final rain point cloud.


### Update the scene processing code for the second stage of DRET on May 9, 2024
By running DERP_rain_simulation.py and changing it to the path of the rain particle txt file.

*Note: Part of this code is referenced from [Fog Simulation work](https://github.com/MartinHahner/LiDAR_fog_sim)*


## SRKD

### config

please just refer to [OpenPCDet](https://github.com/open-mmlab/OpenPCDet) and [SPRAY](https://github.com/Wachemanston/Reconstruction-and-Synthesis-of-Lidar-Point-Clouds-of-Spray), or we will release more detailed guidance later.

###  train
* **multi-GPUs**

`bash scripts/dist_train.sh num_gpus --cfg_file your_cfg --batch_size your_batch --workers your_worker --epochs your_epoch --extra_tag your_tag`

**for example**

`bash scripts/dist_train.sh 2 --cfg_file cfgs/waymo_models/pv_rcnn_plusplus_KD.yaml --batch_size 4 --workers 2 --epochs 50 --extra_tag "pv_rcnn++_fullKD"`

* **single-GPU**


`python3 train.py --cfg_file cfgs/waymo_models/pv_rcnn_plusplus_KD.yaml --batch_size 4 --workers 2 --epochs 50 --extra_tag "pv_rcnn++_fullKD"`

### test
* **all_ckpt**


`/usr/bin/python3 test.py --cfg_file cfgs/waymo_models/pv_rcnn_plusplus_KD.yaml --batch_size 2 --extra_tag 'pv_rcnn++_fullKD' --eval_all`

* **single_ckpt**

 
`test.py --cfg_file cfgs/waymo_models/pv_rcnn_plusplus_KD.yaml --batch_size 2 --extra_tag  'pv_rcnn++_fullKD_delincar_4gpus' --ckpt /home/hx/OpenPCDet/output/waymo_models/pv_rcnn_plusplus_KD/pv_rcnn++_fullKD_delincar_4gpus/ckpt/checkpoint_epoch_27.pth`
