# ==================================================================
# Initial setup
# ------------------------------------------------------------------
# Jammy comes with 3.10 so let's keep life simple by using that dist.
FROM ubuntu:22.04
RUN yes| unminimize

# Set ENV variables
ENV LANG C.UTF-8
ENV SHELL=/bin/bash
ENV DEBIAN_FRONTEND=noninteractive

ENV APT_INSTALL="apt-get install -y --no-install-recommends"
ENV PIP_INSTALL="python3 -m pip --no-cache-dir install --upgrade"
ENV GIT_CLONE="git clone --depth 10"


# ==================================================================
# Tools
# ------------------------------------------------------------------

RUN apt-get update && \
    $APT_INSTALL \
    apt-utils \
    gcc \
    make \
    pkg-config \
    apt-transport-https \
    build-essential \
    ca-certificates \
    wget \
    rsync \
    git \
    vim \
    mlocate \
    libssl-dev \
    curl \
    openssh-client \
    unzip \
    unrar \
    zip \
    csvkit \
    emacs \
    joe \
    jq \
    dialog \
    man-db \
    manpages \
    manpages-dev \
    manpages-posix \
    manpages-posix-dev \
    nano \
    iputils-ping \
    sudo \
    ffmpeg \
    libsm6 \
    libxext6 \
    libboost-all-dev \
    cifs-utils \
    software-properties-common \
    python3-pip


# distro ver (22) comes with a very slow conflict resolution system
RUN pip install -U pip

# ==================================================================
# Installing CUDA packages (CUDA Toolkit 12.1.0 & CUDNN 8.9.0)
# ------------------------------------------------------------------

# Based on https://developer.nvidia.com/cuda-toolkit-archive
# Based on https://developer.nvidia.com/rdp/cudnn-archive
# Based on https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html#package-manager-ubuntu-install

# Installing CUDA Toolkit

RUN wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin && \
    mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600 && \
    wget https://developer.download.nvidia.com/compute/cuda/12.1.0/local_installers/cuda-repo-ubuntu2204-12-1-local_12.1.0-530.30.02-1_amd64.deb && \
    dpkg -i cuda-repo-ubuntu2204-12-1-local_12.1.0-530.30.02-1_amd64.deb && \
    cp /var/cuda-repo-ubuntu2204-12-1-local/cuda-*-keyring.gpg /usr/share/keyrings/ && \
    apt-get update && \
    apt-get -y install cuda

ENV PATH=$PATH:/usr/local/cuda-12.1/bin
ENV LD_LIBRARY_PATH=/usr/local/cuda-12.1/lib64

# Installing CUDNN
RUN wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin && \
    mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600 && \
    apt-get install dirmngr -y && \
    apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/3bf863cc.pub && \
    add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/ /" && \
    apt-get update && \
    apt-get install libcudnn8=8.9.0.*-1+cuda12.1 -y && \
    apt-get install libcudnn8-dev=8.9.0.*-1+cuda12.1 -y && \
    rm /etc/apt/preferences.d/cuda-repository-pin-600

# ==================================================================
# PyTorch
# ------------------------------------------------------------------
# Based on https://pytorch.org/get-started/locally/
RUN $PIP_INSTALL torch torchvision torchaudio torchtext
# ==================================================================
# JAX
# ------------------------------------------------------------------
# Based on https://github.com/google/jax#pip-installation-gpu-cuda
RUN $PIP_INSTALL "jax[cuda12_cudnn88]" -f https://storage.googleapis.com/jax-releases/jax_cuda_releases.html && \
    $PIP_INSTALL flax==0.6.3 && \
    # ==================================================================
    # TensorFlow
    # ------------------------------------------------------------------
    # Based on https://www.tensorflow.org/install/pip
    $PIP_INSTALL tensorflow
# ==================================================================
# Hugging Face
# ------------------------------------------------------------------
# Based on https://huggingface.co/docs/transformers/installation
# Based on https://huggingface.co/docs/datasets/installation
RUN $PIP_INSTALL datasets
# ==================================================================
# JupyterLab
# ------------------------------------------------------------------
# Based on https://jupyterlab.readthedocs.io/en/stable/getting_started/installation.html#pip
RUN $PIP_INSTALL jupyterlab==4.0.9
# ==================================================================
# Additional Python Packages
# ------------------------------------------------------------------
# cython and pyyaml causing some issue
# https://github.com/yaml/pyyaml/issues/601
RUN pip install "cython<3.0.0" wheel && pip install "pyyaml==5.4.1" --no-build-isolation

RUN $PIP_INSTALL \
    numpy==1.26.2 \
    scipy==1.11.4 \
    pandas==2.1.4 \
    cloudpickle==3.0.0 \
    scikit-image \
    matplotlib==3.8.2 \
    ipython==8.18.1 \
    ipykernel==6.27.1 \
    ipywidgets==8.1.1 \
    tqdm==4.66.1 \
    gdown \
    xgboost==2.0.2 \
    pillow==10.1.0 \
    seaborn==0.13.0 \
    sqlalchemy==2.0.23 \
    spacy==3.7.2 \
    nltk==3.8.1 \
    boto3==1.34.1 \
    tabulate==0.9.0 \
    future==0.18.3 \
    # gradient==2.0.6 \
    jsonify==0.5 \
    opencv-python==4.8.1.78 \
    sentence-transformers==2.2.2 \
    wandb==0.16.1 \
    awscli==1.32.1 \
    jupyterlab-snippets==0.4.1 \
    tornado==6.4
# ==================================================================
# Installing transformers & scikit image (fixed)
# ------------------------------------------------------------------
RUN pip install -U git+https://github.com/huggingface/transformers

RUN pip install --pre scikit-learn
# ==================================================================
# Installing JRE and JDK
# ------------------------------------------------------------------

RUN $APT_INSTALL \
    default-jre \
    default-jdk


# ==================================================================
# Node.js and Jupyter Notebook Extensions
# ------------------------------------------------------------------

RUN curl -sL https://deb.nodesource.com/setup_18.x | bash  && \
    $APT_INSTALL nodejs  && \
    $PIP_INSTALL jupyter_contrib_nbextensions jupyterlab-git && \
    jupyter contrib nbextension install --user


# ==================================================================
# Startup
# ------------------------------------------------------------------

EXPOSE 8888 6006

CMD jupyter lab --allow-root --ip=0.0.0.0 --no-browser --ServerApp.trust_xheaders=True --ServerApp.disable_check_xsrf=False --ServerApp.allow_remote_access=True --ServerApp.allow_origin='*' --ServerApp.allow_credentials=True