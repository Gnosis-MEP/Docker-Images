FROM python:3.7

ARG SIT_PYPI_USER_VAR
ARG SIT_PYPI_PASS_VAR

ENV PIPENV_VERSION_TO_INSTALL="2018.11.26"
ENV PIP_VERSION_TO_INSTALL="18.1"
ENV SIT_PYPI_USER $SIT_PYPI_USER_VAR
ENV SIT_PYPI_PASS $SIT_PYPI_PASS_VAR

WORKDIR /service
ADD ./Pipfile /service/Pipfile

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
        libpq-dev \
    && rm -rf /var/lib/apt/lists/* && \
    pip install -U pip==${PIP_VERSION_TO_INSTALL} && \
    pip install -U pipenv==${PIPENV_VERSION_TO_INSTALL} && \
    rm -f Pipfile.lock && pipenv lock -vvv && pipenv --rm && \
    pipenv install --system  && \
    rm -rf /tmp/pip* /root/.cache