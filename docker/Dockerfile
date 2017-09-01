FROM nvidia/cuda:8.0-devel-ubuntu16.04
LABEL maintainer="Kevin Angstadt <angstadt@umich.edu>" \
      description="MNCaRT is an open-source automata processing ecosystem"

# make sure that we have reasonable packages installed
RUN apt-get update && \
    apt-get -y --no-install-recommends install \
        bison \
        build-essential \
        ca-certificates \
        cmake \
        emacs24-nox \
        flex \
        git \
        less \
        libboost-all-dev \
        libpcap-dev \
        libsqlite3-dev \
        nano \
        nasm \
        ocaml-nox \
        ocaml-native-compilers \
        pkg-config \
        ragel \
        sqlite3 \
        sudo \
        vim \
        wget \
        witty \
        witty-dev \
        zip

# create ``mncart`` user with the password ``mncart``
RUN useradd -m mncart && \
    echo mncart:mncart | chpasswd && \
    echo 'mncart ALL=(root) ALL' >> /etc/sudoers

USER mncart
WORKDIR /home/mncart

# make a ~/bin directory
RUN mkdir bin

# download and set up hyperscan
ARG hs_build_threads=8
RUN git clone https://github.com/01org/hyperscan && \
    cd hyperscan && \
    git checkout release-4.4 && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make -j${hs_build_threads}

# download and set up VTR
ARG vtr_build_threads=8
RUN git clone https://github.com/verilog-to-routing/vtr-verilog-to-routing && \
    cd vtr-verilog-to-routing && \
    make -j${vtr_build_threads} && \
    ln -s /home/mncart/vtr-verilog-to-routing/vpr/vpr /home/mncart/vpr

# Download MNCaRT
RUN git clone https://github.com/kevinaangstadt/MNCaRT && \
    cd MNCaRT && \
    git submodule init && \
    git submodule update

# set up each portion of MNCaRT
WORKDIR /home/mncart/MNCaRT

# MNRL
RUN cd MNRL/C++ && \
    echo "export LD_LIBRARY_PATH=/home/mncart/MNCaRT/MNRL/C++:$LD_LIBRARY_PATH" >> /home/mncart/.profile && \
    make

# HSCompile
RUN cd HSCompile && \
    mkdir build && \
    cd build && \
    cmake -DHS_SOURCE_DIR=/home/mncart/hyperscan -DMNRL_SOURCE_DIR=/home/mncart/MNCaRT/MNRL/C++ .. && \
    make && \
    ln -s /home/mncart/MNCaRT/HSCompile/build/hscompile /home/mncart/bin/hscompile && \
    ln -s /home/mncart/MNCaRT/HSCompile/build/hsrun /home/mncart/bin/hsrun && \
    ln -s /home/mncart/MNCaRT/HSCompile/build/pcre2mnrl /home/mncart/bin/pcre2mnrl 

# VASim
RUN cd VASim && \
    make && \
    ln -s /home/mncart/MNCaRT/VASim/vasim /home/mncart/bin/vasim

# RAPID
RUN cd RAPID && \
    make && \
    ln -s /home/mncart/MNCaRT/RAPID/rapid /home/mncart/bin/rapid

# set up the exec to enter with
ENTRYPOINT ["/bin/bash", "-l"]
    
