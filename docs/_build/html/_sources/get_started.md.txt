## Prerequisites

- Linux or macOS (Windows is in experimental support)
- Python 3.6+
- PyTorch 1.3+
- CUDA 9.2+ (If you build PyTorch from source, CUDA 9.0 is also compatible)
- GCC 5+
- opencv-python 4.5.1+
- [MMCV 1.3.14+](https://mmcv.readthedocs.io/en/latest/#installation)
- [MMDetection 2.18+](https://mmcv.readthedocs.io/en/latest/#installation)

## Installation

### Prepare environment

1. Create a conda virtual environment and activate it.

    ```shell
    conda create -n mmrotate python=3.7 -y
    conda activate mmrotate
    ```

2. Install PyTorch and torchvision following the [official instructions](https://pytorch.org/), e.g.,

    ```shell
    conda install pytorch torchvision -c pytorch
    ```

    Note: Make sure that your compilation CUDA version and runtime CUDA version match.
    You can check the supported CUDA version for precompiled packages on the [PyTorch website](https://pytorch.org/).

    `E.g.1` If you have CUDA 10.1 installed under `/usr/local/cuda` and would like to install
    PyTorch 1.5, you need to install the prebuilt PyTorch with CUDA 10.1.

    ```shell
    conda install pytorch cudatoolkit=10.1 torchvision -c pytorch
    ```

    `E.g. 2` If you have CUDA 9.2 installed under `/usr/local/cuda` and would like to install
    PyTorch 1.3.1., you need to install the prebuilt PyTorch with CUDA 9.2.

    ```shell
    conda install pytorch=1.3.1 cudatoolkit=9.2 torchvision=0.4.2 -c pytorch
    ```

    If you build PyTorch from source instead of installing the prebuilt package,
    you can use more CUDA versions such as 9.0.

### Install MMDetection

It is recommended to install MMDetection with [MIM](https://github.com/open-mmlab/mim),
which automatically handle the dependencies of OpenMMLab projects, including mmcv and other python packages.

```shell
pip install openmim
mim install mmdet
```

Or you can still install MMDetection manually:

1. Install mmcv-full.

    ```shell
    pip install mmcv-full -f https://download.openmmlab.com/mmcv/dist/{cu_version}/{torch_version}/index.html
    ```

    Please replace `{cu_version}` and `{torch_version}` in the url to your desired one. For example, to install the latest `mmcv-full` with `CUDA 11.0` and `PyTorch 1.7.0`, use the following command:

    ```shell
    pip install mmcv-full -f https://download.openmmlab.com/mmcv/dist/cu110/torch1.7.0/index.html
    ```

    See [here](https://github.com/open-mmlab/mmcv#installation) for different versions of MMCV compatible to different PyTorch and CUDA versions.

    Optionally you can compile mmcv from source if you need to develop both mmcv and mmdet. Refer to the [guide](https://github.com/open-mmlab/mmcv#installation) for details.

2. Install MMDetection.

    You can simply install mmdetection with the following command:

    ```shell
    pip install mmdet
    ```

    or clone the repository and then install it:

    ```shell
    git clone https://github.com/open-mmlab/mmdetection.git
    cd mmdetection
    pip install -r requirements/build.txt
    pip install -v -e .  # or "python setup.py develop"
    ```

**Note:**

a. When specifying `-e` or `develop`, MMDetection is installed on dev mode
, any local modifications made to the code will take effect without reinstallation.

b. If you would like to use `opencv-python-headless` instead of `opencv-python`,
you can install it before installing MMCV.

c. It is best to use `opencv-python` greater than `4.5.1` because its angle representation has been changed in `4.5.1`. The following experiments are all run with `4.5.3`.

### Install ZeroRotate

You can simply install ZeroRotate with the following command:

```shell
pip install rmmdet
```

or clone the repository and then install it:

```shell
git clone https://github.com/zytx121/mmrotate.git
cd mmrotate
pip install -r requirements.txt
pip install -v -e .
```



### A from-scratch setup script

Assuming that you already have CUDA 10.1 installed, here is a full script for setting up MMDetection with conda.

```shell
conda create -n mmrotate python=3.7 -y
conda activate mmrotate

conda install pytorch==1.6.0 torchvision==0.7.0 cudatoolkit=10.1 -c pytorch -y

# install the latest mmcv
pip install mmcv-full -f https://download.openmmlab.com/mmcv/dist/cu101/torch1.6.0/index.html

# install the latest mmdetection
pip install mmdet==2.18.0

# install ZeroRotate
git clone https://github.com/zytx121/mmrotate.git
cd mmrotate
pip install -r requirements.txt
pip install -v -e .
```