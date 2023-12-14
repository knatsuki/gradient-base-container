FROM paperspace/gradient-base:pt112-tf29-jax0317-py39-20230125

RUN rm -rf /usr/local/bin/python3 /usr/local/bin/python && \
    apt update && \
    $APT_INSTALL python3.11 python3.11-dev  python3.11-venv  python3-distutils-extra && \
    ln -s /usr/bin/python3.11 /usr/local/bin/python3 && ln -s /usr/bin/python3.11 /usr/local/bin/python && \
    curl -sS https://bootstrap.pypa.io/get-pip.py | python3.11 

ENV PATH=$PATH:/root/.local/bin

# COPY requirements.txt .

RUN $PIP_INSTALL tensorflow jupyterlab

EXPOSE 8888 6006

CMD jupyter lab --allow-root --ip=0.0.0.0 --no-browser --ServerApp.trust_xheaders=True --ServerApp.disable_check_xsrf=False --ServerApp.allow_remote_access=True --ServerApp.allow_origin='*' --ServerApp.allow_credentials=True