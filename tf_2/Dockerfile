FROM tensorflow/tensorflow:2.0.0-gpu-py3
MAINTAINER JACOBBIEKER <jacob@bieker.tech>

#ENV http_proxy="http://192.168.0.1:8080"
#ENV https_proxy="https://192.168.0.1:8080"

RUN apt-get clean && apt-get update


# Install wget
RUN apt-get install -y --fix-missing --no-install-recommends wget git

# Install conda
RUN mkdir ~/Downloads \
    && wget https://repo.anaconda.com/miniconda/Miniconda2-4.5.11-Linux-x86_64.sh -nv -O ~/Downloads/Miniconda2-4.5.11-Linux-x86_64.sh
RUN bash ~/Downloads/Miniconda2-4.5.11-Linux-x86_64.sh -b -p /opt/conda

# Install pdal as a conda environment
RUN /opt/conda/bin/conda create --yes --name cpdal-run --channel conda-forge pdal=1.7

# Install tensorflow
RUN pip3 install tensorflow-gpu keras

# Compile and install pointnet2 tensorflow utilities
COPY tf_ops /pointnet2/tf_ops
RUN chmod u+x /pointnet2/tf_ops/compile_ops.sh
RUN cd /pointnet2/tf_ops/ && \
    ./compile_ops.sh

# Set working directory
WORKDIR /pointnet2

# Install laspy, pathlib
RUN apt install -y --fix-missing --no-install-recommends python3-pip
RUN pip3 install laspy numpy

# Install stuff for FACT
RUN pip3 install git+https://github.com/fact-project/photon_stream.git
RUN pip3 install pyfact astropy h5py matplotlib pandas scipy PyYAML
