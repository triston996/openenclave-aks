FROM ubuntu:16.04
RUN apt update; apt -y install wget gnupg python apt-transport-https libssl1.0.0 libssl-dev
RUN echo 'deb [arch=amd64] https://download.01.org/intel-sgx/sgx_repo/ubuntu xenial main' | tee /etc/apt/sources.list.d/intel-sgx.list && wget -qO - https://download.01.org/intel-sgx/sgx_repo/ubuntu/intel-sgx-deb.key | apt-key add -
RUN echo 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/prod xenial main' | tee /etc/apt/sources.list.d/msprod.list && wget -qO - https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN mkdir -p /etc/init; echo "#!/bin/bash" > /sbin/initctl; chmod +x /sbin/initctl
RUN apt update && apt -y install libsgx-enclave-common libsgx-enclave-common-dev libsgx-dcap-ql libsgx-dcap-ql-dev az-dcap-client open-enclave gcc g++
RUN cp -R /opt/openenclave/share/openenclave/samples/helloworld . && . /opt/openenclave/share/openenclave/openenclaverc && cd helloworld && export CC=gcc && export CXX=g++ && make
CMD dpkg --list open-enclave | grep open-enclave; /helloworld/host/helloworldhost /helloworld/enclave/helloworldenc.signed
