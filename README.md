# A Baseline for 3D Multi-Object Tracking 

<img align="center" src="https://github.com/xinshuoweng/AB3DMOT/blob/master/github_demo.gif">

This repository is a python3 version of the original "[A Baseline for 3D Multi-Object Tracking](https://arxiv.org/pdf/1907.03961.pdf)". If you find this code useful, please cite this paper:

```
@article{Weng2019_3dmot, 
  archivePrefix = {arXiv}, 
  arxivId = {1907.03961}, 
  author = {Weng, Xinshuo and Kitani, Kris}, 
  eprint = {1907.03961}, 
  journal = {arXiv:1907.03961}, 
  title = {{A Baseline for 3D Multi-Object Tracking}}, 
  url = {https://arxiv.org/pdf/1907.03961.pdf}, 
  year = {2019} 
}
```

### Introduction
3D multi-object tracking (MOT) is an essential component technology for many real-time applications such as autonomous driving or assistive robotics. However, recent works for 3D MOT tend to focus more on developing accurate systems giving less regard to computational cost and system complexity. In contrast, this work proposes a simple yet accurate real-time baseline 3D MOT system. We use an off-the-shelf 3D object detector to obtain oriented 3D bounding boxes from the LiDAR point cloud. Then, a combination of 3D Kalman filter and Hungarian algorithm is used for state estimation and data association. Although our baseline system is a straightforward combination of standard methods, we obtain the state-of-the-art results. To evaluate our baseline system, we propose a new 3D MOT extension to the official KITTI 2D MOT evaluation along with two new metrics. Our proposed baseline method for 3D MOT establishes new state-of-the-art performance on 3D MOT for KITTI, improving the 3D MOTA from 72.23 of prior art to 76.47. Surprisingly, by projecting our 3D tracking results to the 2D image plane and compare against published 2D MOT methods, our system places 2nd on the official KITTI leaderboard. Also, our proposed 3D MOT method runs at a rate of 214.7 FPS, 65 times faster than the state-of-the-art 2D MOT system. 

### Dependencies:

This code has been tested on python 3.6, and also requires the following packages:
1. scikit-learn
2. filterpy
3. numba
4. matplotlib
5. pillow
6. opencv-python
7. glob2

### 3D Object Detection:
For convenience, we provide the 3D detection of the PointRCNN on the KITTI MOT dataset at ./data/KITTI/3d_det_val (for validation set) and ./data/KITTI/3d_det_test (for test set).

### 3D Multi-Object Tracking (Inference):

To run our tracker on the validation set with the provided detection:

```
$ python main.py 3d_det_val
```
To run our tracker on the test set with the provided detection:

```
$ python main.py 3d_det_test
```
Then, the results will be saved to ./results folder. Note that, please run the code when the CPU is not occupied by other programs otherwise you might not achieve similar speed as reported in our paper.

### 3D Multi-Object Tracking (3D MOT Evaluation):

To reproduce the quantitative results of our 3D MOT system using the proposed KITTI-3DMOT evaluation tool, please run:
  ```
  $ python evaluation/evaluate_kitti3dmot.py 3d_det_val
  ```
Then, the results should be exactly same as below, except for the FPS which depends on the individual machine.

 Method         | AMOTA (%) | AMOTP (%) | MOTA (%) | MOTP (%)| MT (%) | ML (%) | IDS | FRAG | FPS 
--------------- |:---------:|:---------:|:--------:|:-------:|:------:|:------:|:---:|:----:|:---:
 *Ours*         | 39.44     | 74.60     | 76.47    |  78.98  |  69.86 | 7.27   |  0  | 58   | 207.4

### 3D Multi-Object Tracking (Visualization):

To reproduce the qualitative results of our 3D MOT system shown in the paper:

1. Thresholding the trajectories using a proper threshold
2. draw the remaining 3D trajectories on the images (Note that the opencv3 is required by this step, please check the opencv version if there is an error)
  ```
  $ python trk_conf_threshold.py 3d_det_test 
  $ python visualization.py 3d_det_test_thres
  ```

Then, the visualization results are saved to ./results/3d_det_test_thres/trk_image_vis. If one wants to visualize the results on the entire sequences, please first download the KITTI MOT dataset at http://www.cvlibs.net/datasets/kitti/eval_tracking.php and move the image/calibration files to the './data/KITTI' folder.

In addition, one can check out our demo for viusualization in full_demo.mp4


### 3D Multi-Object Tracking (2D MOT Evaluation):
To reproduce the quantitative results of our 3D MOT system using the official KITTI 2D MOT evaluation server, please compress the folder below and upload to http://www.cvlibs.net/datasets/kitti/user_submit.php
  ```
  $ ./results/3d_det_test_thres/data
  ```
Then, the results should be similar to our entry on the KITTI 2D MOT leaderboard: 

 Method         | MOTA (%) | MOTP (%)| MT (%) | ML (%) | IDS | FRAG | FPS 
--------------- |:--------:|:-------:|:------:|:------:|:---:|:----:|:---:
 *Ours*         | 83.34    |  85.23  | 65.85  | 11.54  |  10 | 222  | 214.7
 
 
## Acknowledgement
Code heaviliy borrowed from [link](https://github.com/xinshuoweng/AB3DMOT)
