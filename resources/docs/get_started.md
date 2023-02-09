# Get Started 

This page will lead you to go through with our data and code.



## Prerequisites

Please follow the steps below to get prepared.

#### 1. Environment

**Step 1.** Create conda environment and activate it.

```
conda create --name hdmap python==3.8
conda activate hdmap
```

**Step 2.** Install PyTorch.

```
pip install torch==1.9.0+cu111 torchvision==0.10.0+cu111 -f https://download.pytorch.org/whl/torch_stable.html
```

**Step 3.** Install MMCV series.

```
# Install mmcv-series
pip install mmcv-full==1.3.9 -f https://download.openmmlab.com/mmcv/dist/cu111/torch1.9.0/index.html
pip install mmdet==2.14.0
pip install mmsegmentation==0.14.1
```

**Step 4.** Install mmdetection3d.

Currently we are usingmmdetection3d of  version 0.17.3 . To install mmdet3d, please first download the releases of 0.17.3 from https://github.com/open-mmlab/mmdetection3d/releases. Then run

```
cd mmdetection3d-0.17.3
pip install -v -e .
```

For more details about installation, please refer to open-mmlab [getting_started.md](https://github.com/open-mmlab/mmdetection3d/blob/master/docs/en/getting_started.md).

**Step 5.** Install other requirements.

```
pip install -r requirements.txt
```



#### 2. Data

See 



## Baseline Model

We provide a baseline model based on [VectorMapNet](https://arxiv.org/abs/2206.08920). 

**Expected Results**

|    Method    |  mAP  | Ped Crossing | Divider | Boundary | Config File | Ckpt |
| :----------: | :---: | :----------: | :-----: | :------: | :---------: | :--: |
| VectorMapNet | 42.09 |    38.87     |  49.23  |  38.15   | [config]()  |      |

**Baseline Training**

Single GPU training

```
python tools/train.py plugin/configs/vectormapnet.py
```

Multi GPU training

```
bash tools/dist_train.sh plugin/configs/vectormapnet.py ${NUM_GPUS}
```



## Evaluation

For details of evaluation metrics, please see [metrics.md]().

**Evaluating a Checkpoint**

Single GPU evaluation

```
python tools/test.py plugin/configs/vectormapnet.py ${CHECKPOINT} --eval
```

Multi GPU evaluation

```
bash tools/dist_test.py plugin/configs/vectormapnet.py ${CHECKPOINT} ${NUM_GPUS} --eval
```







