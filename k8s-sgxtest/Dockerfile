FROM ubuntu:18.04

## Install tools required to install openenclave
RUN apt update; apt -y install wget gnupg python apt-transport-https libssl1.0.0 libssl-dev

## Adding the Intel and Microsoft repositories to apt
RUN echo 'deb [arch=amd64] https://download.01.org/intel-sgx/sgx_repo/ubuntu bionic main' | tee /etc/apt/sources.list.d/intel-sgx.list \
    && wget -qO - https://download.01.org/intel-sgx/sgx_repo/ubuntu/intel-sgx-deb.key | apt-key add -
RUN echo "deb [arch=amd64] https://packages.microsoft.com/ubuntu/18.04/prod bionic main" | tee /etc/apt/sources.list.d/msprod.list \
    && wget -qO - https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

## Install openenclave & Intel SGX platform software (PSW)
## Install PSW in specific version instead of using the latest version to avoid the compatibility issue with Intel SGX Linux Driver (DCAP driver)
## PSW version: 2.10
## libsgx-dcap-ql version: 1.7
RUN apt update \
    && apt -y install az-dcap-client open-enclave gcc g++ \
    libsgx-qe3-logic=1.7.100.2-bionic1 libsgx-pce-logic=1.7.100.2-bionic1 libsgx-ae-qe3=1.7.100.2-bionic1 \
    libsgx-dcap-ql=1.7.100.2-bionic1 libsgx-dcap-ql-dev=1.7.100.2-bionic1 libsgx-ae-qve=1.7.100.2-bionic1 \
    libsgx-ae-pce=2.10.100.2-bionic1 libsgx-ae-le=2.10.100.2-bionic1\
    libsgx-enclave-common=2.10.100.2-bionic1 libsgx-enclave-common-dev=2.10.100.2-bionic1 \
    libsgx-aesm-launch-plugin=2.10.100.2-bionic1 libsgx-aesm-launch-plugin=2.10.100.2-bionic1 \
    libsgx-launch=2.10.100.2-bionic1 libsgx-epid=2.10.100.2-bionic1 libsgx-quote-ex=2.10.100.2-bionic1 libsgx-urts=2.10.100.2-bionic1

## Copy over the samples, Source openenclaverc, Set up environment variables, Make the helloworld sample
RUN cp -R /opt/openenclave/share/openenclave/samples . \
    && . /opt/openenclave/share/openenclave/openenclaverc \
    && cd samples/helloworld && export CC=gcc && export CXX=g++ && export PKG_CONFIG_PATH=/opt/openenclave/share/pkgconfig/ \
    && make

## Set the command as the helloworld binaries
CMD /samples/helloworld/host/helloworldhost /samples/helloworld/enclave/helloworldenc.signed
