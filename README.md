# Front-End and Back-End Optimization of ORB-SLAM3 and VINS-Fusion
This repo is for ROB 530 Final Project

Abstract: In this project, two typical SLAM algorithm, ORB-SLAM3 and VINS-Fusion are implemented and optimized basing on their structure. Optimization mainly including modifications and additions related to front-end and back-end structure of each method. At the end, the results of various optimization algorithm is compared with its original implementation, following its optimization performance analyzation basing on EuRoC dataset experiments.


Our modified ORB-SLAM3 and VINS-Fusion are collected seperately in its branch in this repo. 

You can setup each algorithm and test using [EuRoc](https://projects.asl.ethz.ch/datasets/doku.php?id=kmavvisualinertialdatasets), [KITTI](https://www.cvlibs.net/datasets/kitti/eval_odometry.php) and other open datasets. 

## Evaluation using [evo](https://github.com/MichaelGrupp/evo)
Install the evaluation software [evo](https://github.com/MichaelGrupp/evo)
```
sudo apt install python-pip 
pip install evo --upgrade --no-binary evo 
```
After installing the evaluation software, evaluate the absolute pose error.
```
evo_ape tum truth.txt before_filter.txt -p -va
evo_ape tum truth.txt after_filter.txt -p -va 
```
Generate the comparison for trajectory by 
```
evo_traj tum --ref=truth.txt before_filter.txt after_filter.txt -p -va 
```
To compare the ASE(Absolute Pose Errors) of results before and after implementing bilateral filter. Make a new directory, evaluate the ASE of both results and save them as zip files in that directory 
```
mkdir results
evo_ape tum truth.txt before.txt -va --plot --plot_mode xyz --save_results results/before.zip
evo_ape tum truth.txt after.txt -va --plot --plot_mode xyz --save_results results/after.zip
```
Generate the ASE comparison plots by 
```
evo_res results/*.zip -p --save_table results/table.csv
```

## Results

Trajactory of modified ORB-SLAM3 and VINS-Fusion on EuRoC MH-05 dataset
![Example Image](media/ORB_MH05_Stereo_IMU.jpeg)
![Example Image](media/VINS-MH05_stetro_IMU.png)

Error of postion and angle degree using ORB-SLAM3 and VINS-Fusion on EuRoC MH-01 and MH-05 datasets

Stereo:
![Example Image](media/Stereo_results.png)

Stereo & IMU:
![Example Image](media/Stereo_IMU_results.png)
