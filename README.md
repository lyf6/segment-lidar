<img src="https://user-images.githubusercontent.com/72500344/210864557-4078754f-86c1-4e7c-b291-73223bdf4e4d.png" alt="logo" width="200"/>

# segment-lidar
[![License](https://img.shields.io/badge/License-BSD_3--Clause-blue.svg)](https://github.com/Yarroudh/ZRect3D/blob/main/LICENSE)
[![Geomatics Unit of ULiege - Development](https://img.shields.io/badge/Geomatics_Unit_of_ULiege-Development-2ea44f)](http://geomatics.ulg.ac.be/)
[![read - documentation](https://img.shields.io/static/v1?label=read&message=documentation&color=orange)](https://yarroudh.gitbook.io/segment-lidar/)

<img src="https://github.com/Yarroudh/segment-lidar/assets/72500344/0b9450b3-3a2a-4644-b61f-3d5deaa3d077" alt="logo" width=200 />

*Python package for segmenting aerial LiDAR data using Segment-Anything Model (SAM) from Meta AI.*

This package is specifically designed for **unsupervised instance segmentation** of **LiDAR data**. It brings together the power of the **Segment-Anything Model (SAM)** developed by [Meta Research](https://github.com/facebookresearch) and the **segment-geospatial** package from [Open Geospatial Solutions](https://github.com/opengeos) to automatize instance segmentation of 3D point cloud data.

![results](https://github.com/Yarroudh/segment-lidar/assets/72500344/089a603b-697e-4483-af1e-3687a79adcc1)

## Installation

We recommand using `Python>=3.9`. First, you need to install `PyTorch`. Please follow the instructions [here](https://pytorch.org/).

Then, you can easily install `segment-lidar` from [PyPI](https://pypi.org/project/segment-lidar/):

```bash
pip install segment-lidar
```

Or, you can install it from source by running the following commands:

```bash
git clone https://github.com/Yarroudh/segment-lidar
cd segment-lidar
python setup.py install
```

Please, note that the actual version is always under tests. If you find any issues or bugs, please report them in [issues](https://github.com/Yarroudh/segment-lidar/issues) section. The second version should implement more advanced features and fonctionalities.

## Documentation

If you are using `segment-lidar`, we highly recommend that you take the time to read the [documentation](https://yarroudh.gitbook.io/segment-lidar/). The documentation is an essential resource that will help you understand the features of the package, as well as provide guidance on how to use it effectively.

## Basic tutorial

A basic tutorial is available [here](https://yarroudh.github.io/segment-lidar/tutorial.html).

You can also refer to [API](https://yarroudh.github.io/segment-lidar/module.html) for more information about different parameters.

### Without ground filtering

```python
from segment_lidar import samlidar, view

viewpoint = view.TopView()

model = samlidar.SamLidar(ckpt_path="sam_vit_h_4b8939.pth")
points = model.read("pointcloud.las")
labels, *_ = model.segment(points=points, view=view, image_path="raster.tif", labels_path="labeled.tif")
model.write(points=points, segment_ids=labels, save_path="segmented.las")
```

### With ground filtering

```python
from segment_lidar import samlidar, view

viewpoint = view.TopView()

model = samlidar.SamLidar(ckpt_path="sam_vit_h_4b8939.pth")
points = model.read("pointcloud.las")
cloud, non_ground, ground = model.csf(points)
labels, *_ = model.segment(points=cloud, view=view, image_path="raster.tif", labels_path="labeled.tif")
model.write(points=points, non_ground=non_ground, ground=ground, segment_ids=labels, save_path="segmented.las")
```

**Note**: The latest version of `segment-lidar` supports defining a custom pinhole camera, with or without interactive visualization, and save the view as an image. Please, refer to the [documentation](https://yarroudh.github.io/segment-lidar/tutorial.html#interactive-mode) for more details.

## Sample data

For testing purposes, you can download a sample here: [pointcloud.las](https://drive.google.com/file/d/16EF2aRSvo8u0pXvwtaQ6sjhP5h0sWw3o/view?usp=sharing).

This data was retrieved from **AHN-4**. For more data, please visit [GeoTiles.nl](https://geotiles.nl/).

## Model checkpoints

Click the links below to download the checkpoint for the corresponding model type.

- `vit_h`: [ViT-H SAM model.](https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth)
- `vit_l`: [ViT-L SAM model.](https://dl.fbaipublicfiles.com/segment_anything/sam_vit_l_0b3195.pth)
- `vit_b`: [ViT-B SAM model.](https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth)

## Docker Image

**segment-lidar** is also available as [Docker image](https://hub.docker.com/r/yarroudh/segment-lidar).

These are the steps to run `segment-lidar` as a Docker container:

1. First pull the image using the <code>docker pull</code> command:

```bash
docker pull yarroudh/segment-lidar
```

2. To run the Docker container and mount your data and script file inside it, use the <code>docker run</code> command with the <code>-v</code> option to specify the path to the host directory and the path to the container directory where you want to mount the data folder. For example:

```bash
docker run -d -v ABSOLUTE_PATH_TO_HOST_DATA:/home/user yarroudh/segment-lidar
```

This command will start a Docker container in detached mode, mount the **ABSOLUTE_PATH_TO_HOST_DATA** directory on the host machine to the **/home/user/data** directory inside the container, and run the <code>yarroudh/segment-lidar</code> image. Do not change the path of the directory inside the container.

3. Find the container ID and copy it. You can use the <code>docker ps</code> command to list all running containers and their IDs.
4. Launch a command inside the container using <code>docker exec</code>, use the container ID or name and the command you want to run. For example:

```bash
docker exec CONTAINER_ID python SCRIPT_FILE
```

5. To copy the output of the command from the container to a local path, use the <code>docker cp</code> command with the container ID or name, the path to the file inside the container, and the path to the destination on the host machine. For example:

```bash
docker cp CONTAINER_ID:/home/user/PATH_TO_OUTPUT PATH_ON_HOST_MACHINE
```

6. Finally, after executing all the commands and copying the results to your local machine, you can stop the Docker container using the <code>docker stop</code> command followed by the container ID or name:

```bash
docker stop CONTAINER_ID
```

## Related repositories

We would like to express our acknowledgments to the creators of:

- [segment-anything](https://github.com/facebookresearch/segment-anything)
- [segment-geospatial](https://github.com/opengeos/segment-geospatial)

Please, visit these repositories for more information about image raster automatic segmentation using SAM from Meta AI.

## License

This software is under the BSD 3-Clause "New" or "Revised" license which is a permissive license that allows you almost unlimited freedom with the software so long as you include the BSD copyright and license notice in it. Please refer to the [LICENSE](https://github.com/Yarroudh/segment-lidar/blob/main/LICENSE) file for more detailed information.

## Citation

The use of open-source software repositories has become increasingly prevalent in scientific research. If you use this repository for your research, please make sure to cite it appropriately in your work. The recommended citation format for this repository is provided in the accompanying [BibTeX citation](https://github.com/Yarroudh/segment-lidar/blob/main/CITATION.bib). Additionally, please make sure to comply with any licensing terms and conditions associated with the use of this repository.

```bib
@misc{yarroudh:2023:samlidar,
  author = {Yarroudh, Anass},
  title = {LiDAR Automatic Unsupervised Segmentation using Segment-Anything Model (SAM) from Meta AI},
  year = {2023},
  howpublished = {GitHub Repository},
  url = {https://github.com/Yarroudh/segment-lidar}
}
```

Yarroudh, A. (2023). *LiDAR Automatic Unsupervised Segmentation using Segment-Anything Model (SAM) from Meta AI* [GitHub repository]. Retrieved from https://github.com/Yarroudh/segment-lidar

## Author

This software was developped by [Anass Yarroudh](https://www.linkedin.com/in/anass-yarroudh/), a Research Engineer in the [Geomatics Unit of the University of Liege](http://geomatics.ulg.ac.be/fr/home.php).
For more detailed information please contact us via <ayarroudh@uliege.be>, we are pleased to send you the necessary information.

-----

Copyright © 2023, [Geomatics Unit of ULiège](http://geomatics.ulg.ac.be/fr/home.php). Released under [BSD-3 Clause License](https://github.com/Yarroudh/segment-lidar/blob/main/LICENSE).
