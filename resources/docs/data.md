# Data

This page introduces how to download our dataset and how to use it.

## Download

#### Sensor Data

| Split | Google Drive <img src="https://ssl.gstatic.com/docs/doclist/images/drive_2022q3_32dp.png" alt="Google Drive" width="18"/> | Baidu Yun <img src="https://nd-static.bdstatic.com/m-static/v20-main/favicon-main.ico" alt="Baidu Yun" width="18"/> | md5 | Size |
| --- | --- | --- | --- | --- |
| train | [image_0](https://drive.google.com/file/d/1jio4Gj3dNlXmSzebO6D7Uy5oz4EaTNTq/view?usp=share_link) | [image_0](https://pan.baidu.com/s/12aV4CoT8znEY12q4M8XFiw?pwd=m204) | 8ade7daeec1b64f8ab91a50c81d812f6 | ~14.0G |
|  | [image_1](https://drive.google.com/file/d/1IgnvZ2UljL49AzNV6CGNGFLQo6tjNFJq/view?usp=share_link) | [image_1](https://pan.baidu.com/s/1SArnlA2_Om9o0xcGd6-EwA?pwd=khx8) | c78e776f79e2394d2d5d95b7b5985e0f | ~14.3G |
|  | [image_2](https://drive.google.com/file/d/1ViEsK5hukjMGfOm_HrCiQPkGArWrT91o/view?usp=share_link) | [image_2](https://pan.baidu.com/s/1ZghG7gwJqFrGxCEcUffp8A?pwd=0xgm) | 4bf09079144aa54cb4dcd5ff6e00cf79 | ~14.2G |
|  | [image_3](https://drive.google.com/file/d/1r3NYauV0JIghSmEihTxto0MMoyoh4waK/view?usp=share_link) | [image_3](https://pan.baidu.com/s/1ogwmXwS9u-B9nhtHlBTz5g?pwd=sqeg) | fd9e64345445975f462213b209632aee | ~14.4G |
|  | [image_4](https://drive.google.com/file/d/1aBe5yxNBew11YRRu-srQNwc5OloyKP4r/view?usp=share_link) | [image_4](https://pan.baidu.com/s/1tMAmUcZH2SzCiJoxwgk87w?pwd=i1au) | ae07e48c88ea2c3f6afbdf5ff71e9821 | ~14.5G |
|  | [image_5](https://drive.google.com/file/d/1Or-Nmsq4SU24KNe-cn9twVYVprYPUd_y/view?usp=share_link) | [image_5](https://pan.baidu.com/s/1sRyrhcSz-izW2U5x3UACSA?pwd=nzxx) | df62c1f6e6b3fb2a2a0868c78ab19c92 | ~14.2G |
|  | [image_6](https://drive.google.com/file/d/1mSWU-2nMzCO5PGF7yF9scoPntWl7ItfZ/view?usp=share_link) | [image_6](https://pan.baidu.com/s/1P3zn_L6EIGUHb43qWOJYWg?pwd=4wei) | 7bff1ce30329235f8e0f25f6f6653b8f | ~14.4G |
| val | [image_7](https://drive.google.com/file/d/19N5q-zbjE2QWngAT9xfqgOR3DROTAln0/view?usp=share_link) | [image_7](https://pan.baidu.com/s/1rRkPWg-zG2ygsbMhwXjPKg?pwd=qsvb) | c73af4a7aef2692b96e4e00795120504 | ~21.0G |
| test | [image_8](https://drive.google.com/file/d/1CvT9w0q8vPldfaajI5YsAqM0ZINT1vJv/view?usp=share_link) | [image_8](https://pan.baidu.com/s/10zjKeuAw350fwTYAeuSLxg?pwd=99ch) | fb2f61e7309e0b48e2697e085a66a259 | ~21.2G |

For files in Google Drive, you can use the following command by replacing [FILE_ID] and [FILE_NAME] accordingly:
```
wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate 'https://docs.google.com/uc?export=download&id=[FILE_ID]' -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')&id=[FILE_ID]" -O [FILE_NAME]
```


#### Annotations

Please download map [annotations](https://drive.google.com/file/d/1NUwFTVZ_HKeOeqe3oJO01PdBJQcZRVhu/view?usp=sharing) and unzip it. Store your data with following structure.

```
└── data
    ├── argoverse2
    |   ├── [segment_id]
    |   |   ├── image
    |   |   |   ├── [camera]
    |   |   |   |   ├── [timestamp].jpg
    |   |   |   |   └── ...
    |   |   |   └── ...
    |   |   
    |   └── ...
    ├── train_annotations.json
    ├── val_annotations.json
    └── test_annotations.json
├── src
├── tools
...
```



## Data Annotation

Here we introduce the format of our data annotations. The range of annotations is 60 x 30 meters in ego-vehicle coordinate system. All polylines will be truncated at this range.

```
train_annotations {
    segment_id <str>: {                                     -- data by segment_id (by log).
        list [                                              -- samples in a segment forms a list, ordered by time
            frame_data (dict) {                             -- single frame sample dict
                "segment_id": str,                          -- segment_id, unique by segment
                "timestamp": str,                           -- timestamp (or token), unique by sample
                "sensor": cams (dict) {                     -- cams info dict
                    "ring_front_center": img_metas {        -- single img meta info dict
                        'image_path': str,                  -- corresponding image file path, *.jpg
                        "intrinsic": 3x3 matrix,            -- camera intrinsic matrix, 3x3
                        "extrinsic": 4x4 matrix             -- ego-to-camera transformation matrix, 4x4
                    }
                    "ring_front_left":  ...,                -- img meta for all surrouding cameras
                    "ring_front_right": ...,
                    "ring_side_left":   ...,
                    "ring_side_right":  ...,
                    "ring_rear_left":   ...,
                    "ring_rear_right":  ...,
                },
                "annotation": anns (dict) {                 -- annotations for map elements, indexed by categories
                    "ped_crossing": list [                  -- list of ped crossing lines
                        line1 (N1 x 4),                     -- line as list of points, each point as (x, y, z, visibility)
                                                                visibility: 1 for visible at current frame, 0 for occluded
                        line2 (N2 x 4),
                        ...
                    ]
                    "divider": list [...],                  -- list of divider lines
                    "boundary": list [...]                  -- list of boundary lines
                },
                "pose": ego_pose (dict) {                   -- vehicle ego pose
                    "ego2global_translation": 3-d vector,   -- ego2global translation, in (x, y, z)
                    "ego2global_rotation": 3x3 matrix,      -- ego2global rotation matrix
                }
            }
        ]             
    }
}
```
