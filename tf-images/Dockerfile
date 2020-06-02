FROM tensorflow/tensorflow:1.12.0-gpu-py3

ARG SIT_PYPI_USER_VAR
ARG SIT_PYPI_PASS_VAR
ENV SIT_PYPI_USER $SIT_PYPI_USER_VAR
ENV SIT_PYPI_PASS $SIT_PYPI_PASS_VAR

ENV PIPENV_VERSION_TO_INSTALL="2018.11.26"
ENV PIP_VERSION_TO_INSTALL="18.1"
ENV PYENV_ROOT "/root/.pyenv"
ENV PATH="${PYENV_ROOT}/bin:${PYENV_ROOT}/shims:$PATH"

ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8
ENV TF_VERSION=1.12.0

ADD ./init_in_bash.sh /root/init_in_bash.sh

# install os libs, most for openCV
RUN apt-get update \
    && apt-get install -y \
        build-essential \
        cmake \
        git \
        wget \
        unzip \
        yasm \
        pkg-config \
        libswscale-dev \
        libtbb2 \
        libtbb-dev \
        libjpeg-dev \
        libpng-dev \
        libtiff-dev \
        libavformat-dev \
        libssl-dev \
        libsm6 \
        libxrender-dev \
    && rm -rf /var/lib/apt/lists/* && \
# install pyenv to easilly install correct version of python 3.6.5
    curl https://pyenv.run | bash && \
    echo ". ~/init_in_bash.sh" >> ~/.profile && \
    . ~/init_in_bash.sh && \
    pyenv install 3.6.5 && \
    pyenv global 3.6.5 system && \
    . ~/init_in_bash.sh && \
    echo ". ~/init_in_bash.sh" >> ~/.bashrc && \
# Ensure correct version of pip and pipenv
    pip install -U pip==${PIP_VERSION_TO_INSTALL} && \
    pip install -U pipenv==${PIPENV_VERSION_TO_INSTALL} && \
# Install tensorflow-gpu with specific tf version
    pip install tensorflow-gpu==${TF_VERSION} && \
# download tf cpu into cached dir if needed in the future
    mkdir /cached_tf_cpu/ && \
    pip download --no-deps -d /cached_tf_cpu/ tensorflow==${TF_VERSION} && \
    rm -rf /tmp/pip* /root/.cache