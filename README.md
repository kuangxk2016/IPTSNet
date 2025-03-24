# IPTSNet: End-to-End Driving with Integrated Perception-Temporal-Spatial Network
Modern autonomous driving systems require robust and adaptive frameworks to process complex perception data from various sensors, such as LiDAR, cameras, and IMUs, while ensuring efficient decision-making in dynamic environments. Existing models often struggle to integrate perception, temporal reasoning, and spatial reasoning in a unified manner, leading to suboptimal performance in real-world scenarios. To address these challenges, we propose the Integrated Perception-Temporal-Spatial Network (IPTSN), a novel end-to-end framework that seamlessly integrates multi-modal sensor data with advanced temporal and spatial reasoning capabilities.

## Setup
Clone the repo, setup CARLA 0.9.10.1, and build the conda environment:
```Shell
git clone https://github.com/kuangxk2016/IPTSNet.git
cd IPTSNet
chmod +x setup_carla.sh
./setup_carla.sh
conda env create -f environment.yml
conda activate iptsn
easy_install carla/PythonAPI/carla/dist/carla-0.9.10-py3.7-linux-x86_64.egg
pip install torch-scatter -f https://data.pyg.org/whl/torch-1.11.0+cu113.html
pip install mmcv-full==1.5.3 -f https://download.openmmlab.com/mmcv/dist/cu113/torch1.11.0/index.html
```

## Dataset
Dataset Link:  Coming Soon

The dataset is structured as follows:
```
- Scenario
    - Town
        - Route
            - rgb: camera images
            - lidar: 3d point cloud in .npy format
            - semantics: corresponding segmentation images
            - topdown: topdown segmentation maps
            - label_raw: 3d bounding boxes for vehicles
            - measurements: contains ego-agent's position, velocity and other metadata
```

### Data genetation
To generate data, the first step is to launch a CARLA server:
```Shell
./CarlaUE4.sh --world-port=2000 -opengl
```
Once the server is running, use the script below for generating training data:
```Shell
./leaderboard/scripts/datagen.sh <carla root> <working directory>
```

## Training
```Shell
cd IPTSNet
CUDA_VISIBLE_DEVICES=0,1,2,3 torchrun train.py --root_dir /path/to/dataset_root/ --parallel_training 1
```

## Evaluation
Pre-trained weight files should be saved in ./model_ckpt/.
Firstly, launching a CARLA server:
```Shell
./CarlaUE4.sh --world-port=2000 -opengl
```
When the CARLA server is running, evaluate with the script:
```Shell
./leaderboard/scripts/local_evaluation.sh <carla root> <working directory>
```

## Acknowledgments
Parts of this project page were adopted from the [Nerfies](https://nerfies.github.io/) page.

## Website License
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
