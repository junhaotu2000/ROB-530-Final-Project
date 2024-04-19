# Installation guide for ORB-SLAM3
# 1. Installation of all library dependencies
```shell

sudo add-apt-repository "deb http://security.ubuntu.com/ubuntu xenial-security main"
sudo apt update

sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev

sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev libjasper-dev

sudo apt-get install libglew-dev libboost-all-dev libssl-dev

sudo apt install libeigen3-dev

```
---

# 2. Install OpenCV 3.2.0 & Pangolin & boost
### Install OpenCV
```shell
cd ~
mkdir Dev && cd Dev
git clone https://github.com/opencv/opencv.git
cd opencv
git checkout 3.2.0
```
Put the following at the top of header file `gedit ./modules/videoio/src/cap_ffmpeg_impl.hpp`  
`#define AV_CODEC_FLAG_GLOBAL_HEADER (1 << 22)`  
`#define CODEC_FLAG_GLOBAL_HEADER AV_CODEC_FLAG_GLOBAL_HEADER`  
`#define AVFMT_RAWPICTURE 0x0020`  
and save and close the file
```shell
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=Release -D WITH_CUDA=OFF -D CMAKE_INSTALL_PREFIX=/usr/local ..
make -j 3 #adjust the number based on your GPU
sudo make install
```

### Install Pangolin
```shell
cd ~/Dev
git clone https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin 
mkdir build 
cd build 
cmake .. -D CMAKE_BUILD_TYPE=Release 
make -j 3 #adjust the number based on your GPU
sudo make install
```

### Install Boost
```shell
#download from official site: https://www.boost.org/users/download/
#target version is: boost_1_79_0.tar.gz
#unzip the file to Dev (where you clone the OpenCV and Pangolin)
cd ~/Dev
cd boost_1_79_0
sudo ./bootstrap.sh
sudo ./b2 install
```

---

# 3. Install GTSAM 4.0.0
```shell
#Firstly, download the required gtsam version from link: https://github.com/borglab/gtsam/archive/4.0.0-alpha2.zip
#Secondly, unzip the file to folder "Dev" (where you clone the OpenCV and Pangolin)
cd ~/Dev
cd gtsam #change the name corresponding to your folder name
mkdir build
cd build
cmake ..
sudo make install
```
---


# 4. Install ORB-SLAM3
```shell
cd ~/Dev
git clone https://github.com/UZ-SLAMLab/ORB_SLAM3.git 
cd ORB_SLAM3
```
We need to change the header file `gedit ./include/LoopClosing.h` at line 51  
from  
`Eigen::aligned_allocator<std::pair<const KeyFrame*, g2o::Sim3> > > KeyFrameAndPose;`  
to  
`Eigen::aligned_allocator<std::pair<KeyFrame *const, g2o::Sim3> > > KeyFrameAndPose;`
in order to make this comiple.  
Now, we can comiple ORB-SLAM3 and it dependencies as DBoW2 and g2o.  
```shell
chmod +x build.sh
./build.sh
```
---

# 5. Prepare for dataset & run simulations
```shell
#here we use MH_01_easy dataset
#Download the dataset from link: https://projects.asl.ethz.ch/datasets/doku.php?id=kmavvisualinertialdatasets.
#Make a folder named "dataset" in ORB-SLAM3, and unzip the dataset into this folder.
#open terminal in ORB-SLAM folder

./Examples/Monocular/mono_euroc ./Vocabulary/ORBvoc.txt ./Examples/Monocular/EuRoC.yaml ./dataset/MH_01_easy ./Examples/Monocular/EuRoC_TimeStamps/MH01.txt
```
