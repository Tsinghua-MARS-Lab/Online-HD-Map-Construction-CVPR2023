# Get Started 

This page will lead you to go through our data and code.



## Prerequisites

Please follow the steps below to get prepared.

#### 1. Environment

**Step 1.** Create conda environment and activate it.

```
conda create --name hdmap python=3.8
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

Please see [data.md](./data.md).



## Baseline Model

We provide a baseline model based on [VectorMapNet](https://arxiv.org/abs/2206.08920) ([Checkpoint](https://drive.google.com/file/d/16D1CMinwA8PG1sd9PV9_WtHzcBohvO-D/view?usp=sharing)).

**Expected Results**

|    Method    |  mAP  | Ped Crossing | Divider | Boundary | Config File | Ckpt |
| :----------: | :---: | :----------: | :-----: | :------: | :---------: | :--: |
| VectorMapNet | 42.09 |    38.87     |  49.23  |  38.15   | [config]()  |      |

**Baseline Training**

Single GPU training

```
python tools/train.py src/configs/vectormapnet.py
```

Multi GPU training

```
bash tools/dist_train.sh src/configs/vectormapnet.py ${NUM_GPUS}
```



## Evaluation

For details of evaluation metrics, please see [metrics.md](./metrics.md).

**Evaluating a Checkpoint**

Single GPU evaluation

```
python tools/test.py src/configs/vectormapnet.py ${CHECKPOINT} --split val --eval
```

Generate a submission file on test set without evaluation:

```
python tools/test.py src/configs/vectormapnet.py ${CHECKPOINT} --split test --format-only
```

Multi GPU evaluation

```
bash tools/dist_test.py src/configs/vectormapnet.py ${CHECKPOINT} ${NUM_GPUS} --split val --eval
```

Tips for options:

1. Use `--split` option to specify the dataset split to test.
2. Use `--eval` option to generate a submission file and run evaluation code. Only available when you are testing on validation set (with `--split val` set).
3. Use `--format-only` option to generate a submission file without evaluation. By setting `--split test --format-only` you will get a submission file which can be directly submitted to our test server.



**Evaluating a submission file on validation set**

If you want evaluate a submission file (only available on validation set since you do not have annotations on test set), run

```
python tools/evaluate_submission.py ${SUBMISSION_FILE} ${VAL_ANNOTATION_FILE}
```



## Visualization

Visualization tools can be used to visualize ground-truth labels and model's prediction results. Map polylines will be projected (with z-axis set to 0 if missing) and rendered on surrounding camera images. A 2D BEV map view will also be rendered.



To visualize ground-truth labels of a log specified by `${LOG_ID} `

```
python tools/visualization/visualize.py ${LOG_ID} 
```

To visualize also prediction results and filter out all predictions with score lower than `${THR}`

```
python tools/visualization/visualize.py ${LOG_ID} --result ${SUBMISSION_FILE} --thr ${THR}
```

You can specify where to save these rendered images by setting `--out-dir`, default to `demo/`.
