# Based off of
# - https://stackoverflow.com/questions/75467166/i-want-to-switch-python-versions-in-paperspace
# - https://github.com/gradient-ai/base-container/blob/main/all-pt200-tf212-jax048-py311/Dockerfile


FROM paperspace/gradient-base:pt112-tf29-jax0317-py39-20230125

RUN rm -rf /usr/local/bin/python3 /usr/local/bin/python && \
    apt update && \
    $APT_INSTALL python3.11 python3.11-dev  python3.11-venv  python3-distutils-extra && \
    ln -s /usr/bin/python3.11 /usr/local/bin/python3 && ln -s /usr/bin/python3.11 /usr/local/bin/python && \
    curl -sS https://bootstrap.pypa.io/get-pip.py | python3.11 

ENV PATH=$PATH:/root/.local/bin

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
RUN $PIP_INSTALL jupyterlab
# ==================================================================
# Additional Python Packages
# ------------------------------------------------------------------
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
    cython==3.0.6 \
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

RUN pip install -U \ 
    openai==1.4.0 \
    langchain==0.0.350 \
    gradio==4.10.0

EXPOSE 8888 6006

CMD jupyter lab --allow-root --ip=0.0.0.0 --no-browser --ServerApp.trust_xheaders=True --ServerApp.disable_check_xsrf=False --ServerApp.allow_remote_access=True --ServerApp.allow_origin='*' --ServerApp.allow_credentials=True