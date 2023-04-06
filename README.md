
```


```

## Installation

Our environments: ubuntu18.04/GCC7.5/GPU: RTX2080ti.

Our code is tested on:
* CMake 3.10.0
* Eigen 3.3.7
* NVIDIA CUDA 10.2
* OpenCV 4.5.5
* pytorch 1.6.0
* pcl 1.8.1
* ceres 1.14
* Sophus 1.0
## build on linux:
####### Build Raft.
sudo apt-get install libncurses5-dev\
virtualenv python-environment\
source python-environment/bin/activate\
pip install torch==1.6.0 torchvision==0.7.0\
pip install matplotlib tensorboard scipy opencv-python
####### Raft could be run here. Test it
python demo.py --model=models/raft-things.pth --path=demo-frames
####### Provide numpy headers to C++
ln -s `python -c "import numpy as np; print(np.__path__[0])"`/core/include/numpy include/raft/ || true 
####### Install ceres 's dependency 
sudo apt-get install libvtk7-dev libsuitesparse-dev liblapack-dev libblas-dev libgtk2.0-dev
####### Build ceres 's dependency
gflags 2.2.2 set 'BUILD_SHARED_LIBS' to ON\
glog 0.6.0\
boost 1.65
####### Build ceres
ceres 1.14.0
####### Build Opt
    git clone https://github.com/niessner/Opt.git
    as in dynamic fusion
Optional:

* Pangolin

```
mkdir build && cd build
cmake -DVISUALIZATION=ON ..
make -j8
```

We use -DVISUALIZATION=OFF/ON to switch visualization plug.

## Usage

* Download datasets from BundleFusion project mainpage [http://graphics.stanford.edu/projects/bundlefusion/](http://graphics.stanford.edu/projects/bundlefusion/) and unzip it.
* Run Commands:

```
cd build
./bundle_fusion_example ../zParametersDefault.txt ../zParametersBundlingDefault.txt ../datasets/apt0
```

A pangolin window will show up and get real time reconstruction  result.

* Save Mesh:

we provide save mesh button at pangoln GUI, you need to specify the save path at zParametersDefault.txt for item "s_generateMeshDir".



## Result

We provide a reconstruction result of dataset [office2](http://graphics.stanford.edu/projects/bundlefusion/data/office2/office2.zip) with Google Drive: [https://drive.google.com/file/d/121rR0_6H_xTpsSsYAHIHV_sZqJjHdN5R/view?usp=sharing](https://drive.google.com/file/d/121rR0_6H_xTpsSsYAHIHV_sZqJjHdN5R/view?usp=sharing)



## Issues

* Pangolin OpenGL error:

<b>Problem:</b>

```
/usr/local/include/pangolin/gl/glsl.h:709:70: error: ‘glUniformMatrix3dv’ was not declared in this scope
     glUniformMatrix3dv( GetUniformHandle(name), 1, GL_FALSE, m.data());
                                                                      ^
/usr/local/include/pangolin/gl/glsl.h: In member function ‘void pangolin::GlSlProgram::SetUniform(const string&, const Matrix4d&)’:
/usr/local/include/pangolin/gl/glsl.h:713:70: error: ‘glUniformMatrix4dv’ was not declared in this scope
     glUniformMatrix4dv( GetUniformHandle(name), 1, GL_FALSE, m.data());
```

<b>Solution:</b>

```
sudo vim /usr/local/include/pangolin/gl/glplatform.h
#goto line#58
#replace "GL/glew.h" with "/usr/include/GL/glew.h"
```

## Contact

contact with fangasfrank #at gmail.com for porting issues.

cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D CMAKE_C_COMPILER=/usr/bin/gcc-7 \
-D INSTALL_PYTHON_EXAMPLES=ON \
-D INSTALL_C_EXAMPLES=ON \
-D OPENCV_ENABLE_NONFREE=ON \
-D BUILD_opencv_python3=ON \
-D WITH_CUDA=ON \
-D WITH_CUDNN=ON \
-D WITH_TBB=ON \
-D OPENCV_DNN_CUDA=ON \
-D ENABLE_FAST_MATH=1 \
-D CUDA_FAST_MATH=1 \
-D CUDA_ARCH_BIN=8.6 \
-D WITH_CUBLAS=1 \
-D OPENCV_GENERATE_PKGCONFIG=ON \
-D OPENCV_EXTRA_MODULES_PATH=/home/maple/projects/opencv_contrib-4.5.5/modules \
-D PYTHON3_EXECUTABLE=/home/maple/anaconda3/envs/MyFusion/bin/python3.6m \
-D PYTHON3_INCLUDE_DIR=/home/maple/anaconda3/envs/MyFusion/include/python3.6m \
-D PYTHON3_LIBRARY=/home/maple/anaconda3/envs/MyFusion/lib/libpython3.6m.so \
-D PYTHON3_NUMPY_INCLUDE_DIRS=/home/maple/anaconda3/envs/MyFusion/lib/python3.6/site-packages/numpy/core/include \
-D PYTHON3_PACKAGES_PATH=/home/maple/anaconda3/envs/MyFusion/lib/python3.6/site-packages \
-D PYTHON_DEFAULT_EXECUTABLE=/home/maple/anaconda3/envs/MyFusion/bin/python3.6m \
-D CUDNN_LIBRARY=/usr/local/cuda-11.5/targets/x86_64-linux/lib/cudnn.so.8.3.1 \
-D CUDNN_INCLUDE_DIR=/usr/local/cuda-11.5/targets/x86_64-linux/include  \
-D CUDA_CUDA_LIBRARY=/usr/local/cuda-11.5/targets/x86_64-linux/lib/stubs/libcuda.so \
-D OPENCV_PYTHON3_INSTALL_PATH=/home/maple/anaconda3/envs/MyFusion/lib/python3.6/site-packages \
-D WITH_WEBP=OFF \
-D WITH_OPENCL=OFF \
-D ETHASHLCL=OFF \
-D ENABLE_CXX11=ON \
-D BUILD_EXAMPLES=OFF \
-D OPENCV_ENABLE_NONFREE=ON \
-D WITH_OPENGL=ON \
-D WITH_GSTREAMER=ON \
-D WITH_V4L=ON \
-D WITH_QT=OFF \
-D BUILD_opencv_python3=ON \
-D BUILD_opencv_python2=OFF \
-D HAVE_opencv_python3=ON   ..



cmake -DCMAKE_BUILD_TYPE=None -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_GPU=ON -DBUILD_apps=ON ..

sudo apt-get install -y build-essential libgl1-mesa-dev libglew-dev libsdl2-dev libsdl2-image-dev libglm-dev libfreetype6-dev libglfw3-dev libglfw3

cmake -DVTK_QT_VERSION=5 -DQT_QMAKE_EXECUTABLE=/opt/Qt5.9.2/5.9.2/gcc_64/bin/qmake -DVTK_Group_Qt=ON -DCMAKE_PREFIX_PATH=/opt/Qt5.9.2/5.9.2/gcc_64/lib/cmake -DBUILD_SHARED_LIBS=ON ..

