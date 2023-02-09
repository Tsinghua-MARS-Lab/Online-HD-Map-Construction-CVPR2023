# Data

This page introduces how to download our dataset and how to use it.

## Download

#### Sensor Data

To be released.

#### Annotations

Please download map annotation files at [ToDo](xxx) and unzip it. Store your data with following structure.

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

Here we introduce the format of our data annotations.

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
