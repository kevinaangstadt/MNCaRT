FROM nvidia/cuda:8.0-devel-ubuntu16.04
LABEL maintainer="Kevin Angstadt <angstadt@umich.edu>" \
      description="MNCaRT is an open-source automata processing ecosystem"

# make sure that we have reasonable packages installed
RUN apt-get update && \
    apt-get -y --no-install-recommends install \
        build-essential \
        ca-certificates \
        cmake \
        emacs24-nox \
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
        zip

# create ``mncart`` user with the password ``mncart``
RUN useradd -m mncart && \
    echo mncart:mncart | chpasswd && \
    echo 'mncart ALL=(root) ALL' >> /etc/sudoers

USER mncart
WORKDIR /home/mncart

ARG hs_build_threads=8

# download and set up hyperscan
RUN git clone https://github.com/01org/hyperscan && \
    cd hyperscan && \
    git checkout release-4.4 && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make -j${hs_build_threads}

# download and build MNCaRT
RUN git clone https://github.com/kevinaangstadt/MNCaRT && \
    cd MNCaRT && \
    git submodule init && \
    git submodule update && \
    cd MNRL/C++ && \
    make && \
    cd - && \
    cd HSCompile && \
    mkdir build && \
    cd build && \
    cmake -DHS_SOURCE_DIR=/home/mncart/hyperscan -DMNRL_SOURCE_DIR=/home/mncart/MNCaRT/MNRL/C++ .. && \
    make && \
    cd ../../RAPID && \
    make && \
    cd ../VASim && \
    make

# link all of the executables to the ~/bin directory
RUN mkdir bin && \
    ln -s /home/mncart/MNCaRT/VASim/vasim /home/mncart/bin/vasim && \
    ln -s /home/mncart/MNCaRT/HSCompile/build/hscompile /home/mncart/bin/hscompile && \
    ln -s /home/mncart/MNCaRT/HSCompile/build/hsrun /home/mncart/bin/hsrun && \
    ln -s /home/mncart/MNCaRT/HSCompile/build/pcre2mnrl /home/mncart/bin/pcre2mnrl && \
    ln -s /home/mncart/MNCaRT/RAPID/rapid /home/mncart/bin/rapid

# set up the exec to enter with
ENTRYPOINT ["/bin/bash", "-l"]
    