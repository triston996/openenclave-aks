FROM ubuntu:18.04

## Upgrade tools required to install openenclave
RUN apt update; apt -y install wget gnupg python apt-transport-https libssl1.0.0 libssl-dev

## install openenclave
RUN echo 'deb [arch=amd64] https://download.01.org/intel-sgx/sgx_repo/ubuntu bionic main' | tee /etc/apt/sources.list.d/intel-sgx.list && wget -qO - https://download.01.org/intel-sgx/sgx_repo/ubuntu/intel-sgx-deb.key | apt-key add -
RUN echo "deb http://apt.llvm.org/bionic/ llvm-toolchain-bionic-7 main" | tee /etc/apt/sources.list.d/llvm-toolchain-bionic-7.list && wget -qO - https://apt.llvm.org/llvm-snapshot.gpg.key | apt-key add -
RUN echo "deb [arch=amd64] https://packages.microsoft.com/ubuntu/18.04/prod bionic main" | tee /etc/apt/sources.list.d/msprod.list && wget -qO - https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN mkdir -p /etc/init; echo "#!/bin/bash" > /sbin/initctl; chmod +x /sbin/initctl

## Update everything
RUN apt update && apt -y install libsgx-enclave-common libsgx-enclave-common-dev libsgx-dcap-ql libsgx-dcap-ql-dev az-dcap-client open-enclave gcc g++

## Make the helloworld sample
RUN cp -R /opt/openenclave/share/openenclave/samples . && . /opt/openenclave/share/openenclave/openenclaverc && cd samples/helloworld && export CC=gcc && export CXX=g++ && export PKG_CONFIG_PATH=/opt/openenclave/share/pkgconfig/ && make

## Set the command as the helloworld binaries
CMD dpkg --list open-enclave | grep open-enclave; /samples/helloworld/host/helloworldhost /samples/helloworld/enclave/helloworldenc.signed
