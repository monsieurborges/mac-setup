# OpenCV

![FFmpeg](../assets/opencv.png?raw=true)

[OpenCV](https://opencv.org) (Open Source Computer Vision Library) is an open source computer vision and machine learning software library.

## UPGRADE OpenCV

1. Uninstall old version:

    ```bash
    CV_DIRS=(
        "/usr/local/share"
        "/usr/local/include"
        "/usr/local/lib"
        "/usr/local/bin"
    )

    sudo -v

    for cv_dir in ${CV_DIRS[@]}; do
        sudo find ${cv_dir} -name "*opencv*" -exec rm -rf {} \;
    done
    ```

2. Remove installation directories:

    ```bash
    rm -rf "${HOME}/opencv"
    rm -rf "${HOME}/opencv_contrib"
    ```

3. Install OpenCV following the instructions below!

## Install OpenCV with Python and Qt 6 support

1. Choose the version. Paste that in the terminal prompt:

    ```bash
    # export OPENCV_VERSION="4.X.X"
    # export OPENCV_VERSION="3.4.X"
    export OPENCV_VERSION="4.6.0"
    ```

2. Easy install using mac-setup:

    ```bash
    bash <(curl -fsSL raw.githubusercontent.com/monsieurborges/mac-setup/master/install) opencv
    ```

3. Install on your own:

    ```bash
    # Installing Opencv dependencies
    brew install cmake pkg-config ceres-solver gdal eigen glog \
                 harfbuzz jpeg libpng libtiff openexr tbb \
                 ffmpeg gstreamer gst-plugins-base gst-plugins-good
    ```

    ```bash
    pyenv shell 3.10.7
    mkvirtualenv opencv
    pip install --upgrade pip
    pip install numpy
    ```

    ```bash
    # Download OpenCV

    # Clone OpenCV project from GitHub
    git clone https://github.com/opencv/opencv "${HOME}/opencv"

    # Clone OpenCV extra modules from GitHub
    git clone https://github.com/opencv/opencv_contrib "${HOME}/opencv_contrib"

    # Create a branch from last stable version
    cd "${HOME}/opencv"
    git checkout tags/${OPENCV_VERSION} -b ${OPENCV_VERSION}

    # Create a branch from last stable version
    cd "${HOME}/opencv_contrib"
    git checkout tags/${OPENCV_VERSION} -b ${OPENCV_VERSION}

    # Create a build directory
    mkdir "${HOME}/opencv/build"
    ```

    ```bash
    # Check Qt PATH
    if ls ${HOME}/Qt*/*/clang_64/bin/qmake 1>/dev/null 2>&1; then
        QT_ON_OFF=ON
    else
        QT_ON_OFF=OFF
    fi

    QT_LIB=$(echo ${HOME}/Qt*/*/clang_64/lib/cmake)
    QT_QMAKE=$(echo ${HOME}/Qt*/*/clang_64/bin/qmake)

    # Check Python PATH
    pyenv shell 3.10.7
    workon opencv
    py3_exec=$(which python)
    py3_config=$(python -c "from distutils.sysconfig import get_config_var as s; print(s('LIBDIR'))")
    py3_include=$(python -c "import distutils.sysconfig as s; print(s.get_python_inc())")
    py3_packages=$(python -c "import distutils.sysconfig as s; print(s.get_python_lib())")
    py3_numpy=$(python -c "import numpy; print(numpy.get_include())")
    ```

    ```bash
    # Configure OpenCV via CMake
    cd "${HOME}/opencv/build"
    pyenv shell 3.10.7
    workon opencv

    cmake -D CMAKE_BUILD_TYPE=RELEASE \
          -D CMAKE_INSTALL_PREFIX="/usr/local" \
          -D OPENCV_EXTRA_MODULES_PATH="${HOME}/opencv_contrib/modules" \
          -D ENABLE_PRECOMPILED_HEADERS=OFF \
          -D OPENCV_GENERATE_PKGCONFIG=ON \
          -D WITH_CUDA=OFF \
          -D WITH_QT="${QT_ON_OFF}" \
          -D CMAKE_PREFIX_PATH="${QT_LIB}" \
          -D QT_QMAKE_EXECUTABLE="${QT_QMAKE}" \
          -D QT5_DIR="${QT_LIB}/Qt5" \
          -D QT5Core_DIR="${QT_LIB}/Qt5Core" \
          -D QT5Gui_DIR="${QT_LIB}/Qt5Gui" \
          -D QT5Test_DIR="${QT_LIB}/Qt5Test" \
          -D QT5Widgets_DIR="${QT_LIB}/Qt5Widgets" \
          -D Qt5Concurrent_DIR="${QT_LIB}/Qt5Concurrent" \
          -D OPENCV_ENABLE_NONFREE=ON \
          -D WITH_GTK=OFF \
          -D WITH_OPENGL=ON \
          -D WITH_OPENVX=ON \
          -D WITH_OPENCL=ON \
          -D WITH_TBB=ON \
          -D WITH_EIGEN=ON \
          -D WITH_GDAL=ON \
          -D WITH_FFMPEG=ON \
          -D BUILD_TIFF=ON \
          -D BUILD_opencv_python2=OFF \
          -D BUILD_opencv_python3=ON \
          -D PYTHON_DEFAULT_EXECUTABLE="${py3_exec}" \
          -D PYTHON3_EXECUTABLE="${py3_exec}" \
          -D PYTHON3_INCLUDE_DIR="${py3_include}" \
          -D PYTHON3_PACKAGES_PATH="${py3_packages}" \
          -D PYTHON3_LIBRARY="${py3_config}" \
          -D PYTHON3_NUMPY_INCLUDE_DIRS="${py3_numpy}" \
          -D INSTALL_PYTHON_EXAMPLES=ON \
          -D INSTALL_C_EXAMPLES=ON \
          -D BUILD_EXAMPLES=ON \
          -D CMAKE_CXX_STANDARD=14 ..
    ```

    ```bash
    # make opencv
    cd "${HOME}/opencv/build"
    pyenv shell 3.10.7
    workon opencv

    make -j 4
    ```

    ```bash
    # make install opencv
    sudo make -j 4 install
    ```

## Test OpenCV installation with Python

```bash
workon opencv
python -c "import cv2;  print(cv2.__version__)"
```

## Test OpenCV installation with C++

We can run some sample code located at `opencv/samples/cpp`:

```bash
cd "${HOME}/opencv/samples/cpp"
workon opencv

g++ -std=c++11 $(pkg-config --cflags --libs opencv4) facedetect.cpp -o ${TMPDIR}/test

${TMPDIR}/test
```

### Test OpenCV installation with Qt

1. Add `:/usr/local/bin` to PATH

    Go to **Project**, expand **Build Environment** and add `:/usr/local/bin` to PATH.

2. Add a new variable called `PKG_CONFIG_PATH` with the value `/usr/local/lib/pkgconfig`

    ![Qt build settings for OpenCV](../assets/qt-path.png?raw=true)

3. Add a few lines to the project file `.pro` to tell qmake to use pkg-config for OpenCV:

    ```bash
    # Set up Qmake to use pkg-config
    QT_CONFIG -= no-pkg-config
    CONFIG += link_pkgconfig
    PKGCONFIG += opencv4

    # Add OpenCV Libraries
    LIBS += $(pkg-config --libs opencv4)
    ```

4. You may receive a runtime error similar to the message below:

    ```bash
    dyld: Symbol not found: __cg_jpeg_resync_to_restart
    Referenced from: /System/Library/Frameworks/ImageIO.framework/Versions/A/ImageIO
    Expected in: /usr/local/lib/libJPEG.dylib
    in /System/Library/Frameworks/ImageIO.framework/Versions/A/ImageIO
    The program has unexpectedly finished.
    ```

    **To fix it**, go to `Project` -> `Run` -> `Run Environment` -> Unset `DYLD_LIBRARY_PATH`.

    ![DYLD problem](../assets/qt-dyld.png?raw=true)

## Sym-link OpenCV to a virtual environment (if necessary)

1. Activate the TARGET Python virtual environment:

    ```bash
    workon TARGET_ENV
    ```

2. Create a symbolic link to OpenCV:

    ```bash
    # Get the OpenCV library path
    opencv_lib=$(echo ${HOME}/opencv/build/lib/python3/*.so)

    # Get the site-packages path of the virtual environment
    site_packages=$(python3 -c "import distutils.sysconfig as s; print(s.get_python_lib())")

    # Create a symbolic link to OpenCV library
    ln -s ${opencv_lib} ${site_packages}
    ```
