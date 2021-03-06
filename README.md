# MLMSNet

Lightweight Multi-Level Multi-Scale Feature Fusion Network for Semantic Segmentation.

Demo video: [link](https://www.bilibili.com/video/BV1iN411o7So/)

# Update

- 2021.05.13 Support 4 encoder: moiblenet large/small and width-multi 0.75/1.0.

- 2021.05.13  Support mlmsnetv2, Use depth separable convolution for ASPP and SE block. (mobilenetv3 large backbone, width-multi 1.0) 713x713 input: Flops 14.2209G, Params 3.5739M.

- 2021.05.30 Support CamVid Dataset. The [link](https://github.com/lih627/CamVid) to download CamVid for our segmentation method.

  

# Dataset Preparation

1. Download Cityscapes images from [Cityscape website](https://www.cityscapes-dataset.com/downloads/). We need **gtFine_trainvaltest.zip (241MB)** and **leftImg8bit_trainvaltest.zip (11GB)** . The **leftImg8bit_demoVideo.zip (6.6GB)** is optional, which is used to generate the demo segmentation results.

2. Download [Cityscapes Scripts](https://github.com/mcordts/cityscapesScripts). Use [**createTrainIdLabelImgs.py**](https://github.com/mcordts/cityscapesScripts/blob/master/cityscapesscripts/preparation/createTrainIdLabelImgs.py) to generate the trainIDs from cityscapes json annotations.

3. link cityscapes dataset to the project **dataset** folder. 

   ```shell
   ln -s path/to/cityscapes  path/to/MLMSNet/dataset/
   ```

   The **dataset** folder structure is list as follows:

   ```
   cityscapes -> path/to/cityscapes
   ├── cityscapes_demo.txt
   ├── cityscapes_test.txt
   ├── demoVideo
   │   ├── stuttgart_00
   │   ├── stuttgart_01
   │   └── stuttgart_02
   ├── fine_train.txt
   ├── fine_val.txt
   ├── gtFine
   │   ├── test
   │   ├── train
   │   └── val
   └── leftImg8bit
       ├── test
       ├── train
       └── val
   ```

4. `cityscapes_demo.txt`,` cityscapes_test.txt`,  `fine_train.txt` ,`fine_val.txt` is in `MLMSNet/misc`

# Train



```shell
cd path/to/MLMSNet

export PYTHONPATH=./

python tool/train.py --config config/cityscapes_ohem_large.yaml  2>&1 | tee ohem_largetrain.log
```

This is the training result using the default configuration parameters, corresponding to `config/cityscapes_ohem_large.yaml`:

```
INFO:main-logger:Val result: mIoU/mAcc/allAcc 0.6695/0.7546/0.9541.
```

# Evaluation

```shell
cd path/to/MLMSNet

export PYTHONPATH=./

python tool/test.py --config config/cityscapes_ohem_large.yaml  2>&1 | tee ohem_large_test.log
```

This is the evaluation result using the default configuration parameters, corresponding to `config/cityscapes_ohem_large.yaml`:

```
Eval result: mIoU/mAcc/allAcc 0.7268/0.7991/0.9540.
```

# Pretrained Model

You can download pretrained models from [Google Drive](https://drive.google.com/drive/folders/1PZ1krdgj9j6FDyJAxUALxXbrTCiWL1_d?usp=sharing).

## Cityscapes

| Model | val mIoU/mAcc/allAcc |config| link |
| ----- | -------------------- |------|-------- |
| MLMS-L      | 0.7268/0.7991/0.9540 |[cityscapes_ohem_large.yaml](./config/cityscapes_ohem_large.yaml)| [MLMS_L](https://drive.google.com/file/d/1JHf9nT4Hgcb3t3HVTd7gZQFN-J1epxym/view?usp=sharing) |
| MLMS-S | 0.7274/0.8033/0.9537 | [cityscapes_ohem_small.yaml](./config/cityscapes_ohem_small.yaml) | [MLMS_S](https://drive.google.com/file/d/1fYJWfeEwx2AhWKedFsopFTguhwBoWUIh/view?usp=sharing) |
| MLMSv2-L | 0.7164/0.7982/0.9526 | [mlmsv2_large.yaml](./config/mlmsv2_large.yaml) | [MLMSv2_L](https://drive.google.com/file/d/1Rb2Q6nu5RyZkbKmDtlEQzFUY-E_8cMl2/view?usp=sharing) |



## CamVid


| Model | val mIoU/mAcc/allAcc |config| link |
| ----- | -------------------- |------|-------- |
| camvid-mlms-l | 0.6814/0.7574/0.9196 |[camvid_ohem_large.yaml](./config/camvid_ohem_large.yaml)| [camvid_mlms_l](https://drive.google.com/file/d/1Oyx1afKj13yP7Vh9YSJn6q6IGjGkgRPV/view?usp=sharing) |
| camvid-mlms-s | 0.6790/0.7612/0.9188 | [camvid_ohem_small.yaml](./config/camvid_ohem_small.yaml) | [camvid_mlms_s](https://drive.google.com/file/d/1MGolivV2z1tAZy6cRUP-0dK2HFfR6PXt/view?usp=sharing) |



# Reference

1. The codebase is from [semseg: Semantic Segmentation in Pytorch](https://github.com/hszhao/semseg):

   ```
   @misc{semseg2019,
     author={Zhao, Hengshuang},
     title={semseg},
     howpublished={\url{https://github.com/hszhao/semseg}},
     year={2019}
   }
   @inproceedings{zhao2017pspnet,
     title={Pyramid Scene Parsing Network},
     author={Zhao, Hengshuang and Shi, Jianping and Qi, Xiaojuan and Wang, Xiaogang and Jia, Jiaya},
     booktitle={CVPR},
     year={2017}
   }
   @inproceedings{zhao2018psanet,
     title={{PSANet}: Point-wise Spatial Attention Network for Scene Parsing},
     author={Zhao, Hengshuang and Zhang, Yi and Liu, Shu and Shi, Jianping and Loy, Chen Change and Lin, Dahua and Jia, Jiaya},
     booktitle={ECCV},
     year={2018}
   }
   ```

2. The MobileNetv3 code and pretrained model is from [mobilenetv3.pytorch: 74.3% MobileNetV3-Large and 67.2% MobileNetV3-Small model on ImageNet](https://github.com/d-li14/mobilenetv3.pytorch)

3. This project gave me a better understanding of the loss function related to the segmentation field: [SegLoss](https://github.com/JunMa11/SegLoss)
