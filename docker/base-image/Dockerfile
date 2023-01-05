# Copyright 2020 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM ubuntu:focal

ENV DEBIAN_FRONTEND=noninteractive

# Python 3.10.8 is not the default version in Ubuntu 20.04 (Focal Fossa).
ENV PYTHON_VERSION 3.10.8
# Install dependencies required by Python3 or Pip3.
RUN apt-get update && \
    apt-get install -y ca-certificates

RUN sed -i "s@http://.*archive.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list && \
    sed -i "s@http://.*security.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list && \
    apt-get update && \
    apt-get install -y software-properties-common && \
    apt-get install --reinstall ca-certificates

RUN add-apt-repository -y ppa:deadsnakes/ppa && \
    sed -i 's/ppa.launchpad.net/launchpad.proxy.ustclug.org/g' /etc/apt/sources.list /etc/apt/sources.list.d/*.list && \
    sed -ri 's#(.*http)(://launchpad.proxy.ustclug.org.*)#\1s\2#g' /etc/apt/sources.list /etc/apt/sources.list.d/*.list && \
    apt-get update && apt-get install -y python3.10 wget

RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.10 0 && \
    update-alternatives --set python3 /usr/bin/python3.10

RUN wget https://bootstrap.pypa.io/get-pip.py && python3 get-pip.py

# RUN apt-get update && \
#     apt-get install -y ca-certificates

# RUN sed -i "s@http://.*archive.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list && \
#     sed -i "s@http://.*security.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list && \
#     apt-get update && \
#     apt-get upgrade -y && \
#     apt-get install -y \
#     curl \
#     xz-utils \
#     build-essential \
#     zlib1g-dev \
#     libssl-dev \
#     libffi-dev

# RUN cd /tmp/ && \
#     curl -O https://www.python.org/ftp/python/$PYTHON_VERSION/Python-$PYTHON_VERSION.tar.xz && \
#     tar -xvf Python-$PYTHON_VERSION.tar.xz > /dev/null && \
#     cd Python-$PYTHON_VERSION && \
#     ./configure \
#         --enable-loadable-sqlite-extensions \
#         --enable-optimizations \
#         > /dev/null && \
#     make -j install > /dev/null && \
#     rm -r /tmp/Python-$PYTHON_VERSION.tar.xz /tmp/Python-$PYTHON_VERSION && \
#     ln -s /usr/local/bin/python3 /usr/local/bin/python && \
#     ln -s /usr/local/bin/pip3 /usr/local/bin/pip

# Install common python dependencies.
COPY ./requirements.txt /tmp
RUN pip3 config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple &&   pip3 install -r /tmp/requirements.txt

# Install dependencies required by google-cloud-cli.
# RUN apt-get update && \
#     apt-get install -y \
#     apt-transport-https \
#     ca-certificates \
#     gnupg

# Install google-cloud-cli (previously named google-cloud-sdk).
# RUN echo "deb https://packages.cloud.google.com/apt cloud-sdk main" \
#     | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && \
#     curl https://packages.cloud.google.com/apt/doc/apt-key.gpg \
#     | apt-key add - && \
#     apt-get update && \
#     apt-get install -y google-cloud-cli
