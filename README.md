﻿<!---
[![Anaconda-Server Badge](https://anaconda.org/bsteubing/activity-browser/badges/version.svg)](https://anaconda.org/bsteubing/activity-browser)
[![Anaconda-Server Badge](https://anaconda.org/bsteubing/activity-browser/badges/downloads.svg)](https://anaconda.org/bsteubing/activity-browser)
[![Pull request tests](https://github.com/LCA-ActivityBrowser/activity-browser/actions/workflows/main.yaml/badge.svg)](https://github.com/LCA-ActivityBrowser/activity-browser/actions/workflows/main.yaml)
[![Coverage Status](https://coveralls.io/repos/github/LCA-ActivityBrowser/activity-browser/badge.svg?branch=master)](https://coveralls.io/github/LCA-ActivityBrowser/activity-browser?branch=master)
-->

# PyHAPT: A Python-based Human Activity Pose Tracking Data Processing Framework

<img src="./src/windows.png"/>

<img src="./src/linux.png"/>

PyHAPT is a novel Python-based Human Activity Pose Tracking video data processing framework (PyHAPT). It provides the functionality to efficiently process annotated human pose tracking raw video data collected in unconstrained environments. 
Besides, PyHAPT provides the functionalities of interpolation to recover the missing joints data and data visualization that gives insights into the spatial-temporal skeletal information. The processed data could be readily used for developing new human activity recognition deep learning models, which could be deployed on mobile service robots.

## Highlights

-	A novel human activity pose tracking dataset pre-precessing framework.
-	Automatically generate multiple human pose detection and tracking pre-processed data from video.
-	Lightweight and easy to use.

<!--
## Demo
Please watch our video on [YouTube](https://youtu.be/iTE9zTa5Bdk) to see how to use HAVPTAT.
-->

## Contents
- [Dependencies](#dependencies)
- [Prerequisites](#Prerequisites)
- [Getting started](#getting-started)
- [Contributing](#contributing)
- [Authors](#authors)
- [License](#license)
- [Citation](#citation)

## Dependences

- Python >= 3.7
- argparse
- numpy as np
- pickle
- os
- tqdm
- matplotlib
- json
- sklearn
- shutil
- pandas
- textwrap
- csv
- sys

## Prerequisites
- Operation System: Windows or Ubuntu 
- IDE: Pycharm
   
Note: To guarantee the correct function of `data_visualization.py` of plot, users need to set the `Pycharm` IDE as:
- [File] - [Settings] - [Tools] - [Python Scientific] - Uncheck `Show plots in tool window`   
  

## Getting started

Clone the project to your file system.

```bash
git clone https://github.com/AIRLab-POLIMI/PyHAPT.git
```

 

## Parameters setting

**main.py**

- `--folder-write`: main root folder of the generated data               
- `--folder-raw-data`: main root folder of the raw data          
- `--defined-label`: csv file with defined action classes 
- `--num-joints`: number of joints is 17 generated by OpenPifPaf                
- `--threshold-valid-frame`: threshold of tot number of the joints as valid pose       
- `--padding-frame`: padded number of frame, e.g., 300

For example:

```
python  main.py
        --folder-write              data
        --folder-raw-data           raw_data
        --defined-label             action_label.csv
        --num-joints                17
        --threshold-valid-frame     10  
        --padding-frame             300
```



**data_visualization.py**

- `--folder-visual`: main root path of the raw data to visualize
- `--json-action`: entire folder action data without recovery missing data
- `--json-action-recovered`: entire folder action data after recovery missing data
- `--number-clip`: number of clip in that folder 
- `--number-sequence`: number of action sequence in that folder

For example:

```
data_visualization.py  
      --folder-visual           data 
      --json-action             cleaning_clip_folder.json
      --json-action-recovered   cleaning_clip_folder.json
      --number-clip             1
      --number-sequence         1
```



## Recommended folder structure
The following folder tree is recommended:

```
[YOUR_HAVPTAT_PROJECT_ROOT_PATH]
│   
│
└─── raw_data (annotated raw data generated by OpenPifPaf of different action folders )
│     │
│     └───── cleaning (user-defined action folder, could be customized)
│     │  │   action_VID_20210305_180331.json (annotated raw data generated by OpenPifPaf in JSON format)
│     │  │   action_VID_20210306_182118.json
│     │  │   action_VID_20210306_182137.json    
│     │  │   ...
│     │  └──────        
│     │
│     └────── crouching (user-defined action folder, could be customized)
│     │   │   action_VID_20210226_153434.json (annotated raw clip data generated by OpenPifPaf in JSON format)
│     │   │   action_VID_20210226_153452.json     
│     │   │   action_VID_20210226_172655.json
│     │   │   ...
│     │   └──────     
│     │   other personalized action folder name 
│     │   ...
│     └──────
│
└─── data (data generated by the script)
│     │
│     └───── action_clip_folder (the same action is combined to the unique .json file without recovery of the data)
│     │  │   cleaning_clip_folder.json   (all the 'cleaning' action data without recovery of the data)
│     │  │   crouching_clip_folder.json  (all the 'crouching' action data without recovery of the data)
│     │  │   ...
│     │  └──────
│     └───── action_clip_recovered_folder (the same action is combined to the unique .json file after having recovered the missing data)
│     │  │   cleaning_clip_folder.json    (all the 'cleaning' action data with recovery of the missing data)
│     │  │   crouching_clip_folder.json   (all the 'crouching' action data with recovery of the missing data) 
│     │  │   ...
│     │  └──────
│     └───── output (data ready-for-use IMPORTANT !!!)
│     │  │   train_data_joint.npy
│     │  │   train_label.pkl    
│     │  │   val_data_joint.npy
│     │  │   val_label.pkl
│     │  └──────
│     └───── output_debug  (debug data ready-for-use, with little samples)
│     │  │   train_data_joint_light.npy
│     │  │   train_label_light.pkl    
│     │  │   val_data_light.npy
│     │  │   val_label_light.pkl
│     │  └──────                
│     │
│     └───── picture  (plot picture folder)
│     │      │
│     │      └───────── comparison (plot comparison picture of recovered and without recovered pose)
│     │      │      │     fig0.png
│     │      │      │     fig1.png
│     │      │      │     ...
│     │      │      └──────
│     │      └───────── recovered (plot picture of with recovered pose)
│     │      │      │     fig0.png
│     │      │      │     fig1.png
│     │      │      │     ...
│     │      │      └──────
│     │      └───────── without_recovered (plot picture of without recovered pose)
│     │            │     fig0.png
│     │            │     fig1.png
│     │            │     ...
│     │            └───────
│     │
│     └───── processing (some intermediate processing files, could be removed)
│             │  clip_global_data.json 
│             │  X_global_data.npy
│             │  X_global_data_to_align.npy 
│             │  Y_global_data.json
│             └───────
└──────────────────────────────────────────────              
```

Users should provide the `raw_data` and its subfolders which store the raw data generated by OpenPifPaf. It is better to store the annotated raw data into different folders using action name as folder name.

The script will generate `data` automatically, the final ready-to-use data are stored in `output` folder.
- train_data_joint.npy
- train_label.pkl    
- val_data.npy
- val_label.pkl 

The lightweight debug version will be stored in `output_debug` folder.  The `picture` folder stores pictures generated by the `'data_visualizartion.py'` script. 
it also stores the recovered missing joints pictures and without recovered missing joints pictures.


## Contributing

**Your contribution counts! The PyHAPT is a community project.**

If you have ideas for improvements to the code or documentation or want to propose new features, please open issues and/or pull-requests.

If you experience problems or are suffering from a specific bug, please [raise an issue](https://github.com/AIRLab-POLIMI/PyHAPT/issues) here on GitHub.

## Authors
- Hao Quan (hao.quan@polimi.it)
- Andrea Bonarini (andrea.bonarini@polimi.it)


## License
This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Lesser General Public License as published
by the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Lesser General Public License for more details.

You should have received a copy of the GNU Lesser General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.


## Citation

Please cite this work if you find it useful:.

    @article{,
        title = {},
        journal = {},
        volume = {},
        pages = {},
        year = {2022},
        issn = {},
        doi = {},
        url = {},
        author = {},
        keywords = {}
    }
