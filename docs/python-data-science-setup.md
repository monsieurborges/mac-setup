# Python for Data Science

![Python packages](../assets/python-package.png?raw=true)

## Install dependencies

```bash
# Leave Python virtual environment
deactivate

# Install Graphviz - Keras dependency
brew install Graphviz
```

## Create a Python virtual environment

```bash
# Example using Python 3.10.7
pyenv shell 3.10.7
mkvirtualenv py310
pip install --upgrade pip
```

## Install TensorFlow

[TensorFlow](https://www.tensorflow.org) is an end-to-end open source platform for machine learning.

```bash
# Activate the virtual environment
workon py310

# Install TensorFlow
if [[ "$(/usr/bin/uname -m)" == "arm64" ]]; then
    # ARM macOS https://developer.apple.com/metal/tensorflow-plugin
    pip install tensorflow-macos
    pip install tensorflow-metal
else
    # Intel macOS
    pip install tensorflow
fi
```

## Install Keras

[Keras](https://keras.io) is a high-level neural networks API, written in Python and capable of running on top of TensorFlow, CNTK, or Theano.

```bash
# Activate the virtual environment
workon py310

# Install Keras
pip install h5py pydot keras
```

## Install Python scientific libraires

```bash
# Activate the virtual environment
workon py310

# Install libraries
pip install numpy \
            pandas \
            scipy \
            pillow \
            scikit-learn \
            scikit-image \
            sk-video
```

## Install basic libraires

```bash
# Activate the virtual environment
workon py310

# Install libraries
pip install autopep8 \
            Cython \
            ipykernel \
            progressbar2 \
            pydocstyle \
            pylint \
            pytest \
            requests \
            markdown
```

## Install Matplotlib

```bash
# Activate the virtual environment
workon py310

# Install Matplotlib
pip install ipympl matplotlib

# Fix Matplotlib backend
mkdir -p "${HOME}/.config/matplotlib"
echo "backend : TkAgg" > "${HOME}/.config/matplotlib/matplotlibrc"
```

## Install Jupyter Lab

1. Install Node

    ```bash
    # Leave Python virtual environment
    deactivate

    # Install Node
    brew install node
    ```

2. Install Jupyter Lab

    ```bash
    # Activate Python environment
    workon py310

    # Install Jupyterlab
    pip install jupyterlab jupyterlab_widgets ipywidgets
    ```

3. Install Jupyter Lab extensions

    ```bash
    # Activate Python environment
    workon py310

    # Install extensions
    pip install tensorboard jupyter-tensorboard jupytext

    # Check extensions status
    jupyter labextension list
    ```
