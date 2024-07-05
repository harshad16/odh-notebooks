ARG BASE_IMAGE
FROM ${BASE_IMAGE}
LABEL name="odh-notebook-rocm-python-3.9" \
      summary="ROCm Python 3.9 base image for ODH notebooks" \
      description="ROCm Python 3.9 builder image based on CentOS Stream 9 for ODH notebooks" \
      io.k8s.display-name="ROCm Python 3.9 base image for ODH notebooks" \
      io.k8s.description="ROCm Python 3.9 builder image based on C9S for ODH notebooks" \
      authoritative-source-url="https://github.com/opendatahub-io/notebooks" \
      io.openshift.build.commit.ref="main" \
      io.openshift.build.source-location="https://github.com/opendatahub-io/notebooks/tree/main/amd/python-3.9" \
      io.openshift.build.image="quay.io/opendatahub/workbench-images:rocm-python-3.9"

USER 0
WORKDIR /opt/app-root/bin

ARG ROCM_VERSION=6.1
ARG AMDGPU_VERSION=6.1
# Base
RUN yum -y install git java-1.8.0-openjdk python; yum clean all && rm -rf /var/cache/yum
# Install ROCm AMD from:
# https://github.com/ROCm/ROCm-docker/blob/master/dev/Dockerfile-centos-7-complete
# Enable epel-release repositories

# Install the ROCm rpms
RUN echo "[ROCm]" > /etc/yum.repos.d/rocm.repo && \
    echo "name=ROCm" >> /etc/yum.repos.d/rocm.repo && \
    echo "baseurl=https://repo.radeon.com/rocm/el9/$ROCM_VERSION/main" >> /etc/yum.repos.d/rocm.repo && \
    echo "enabled=1" >> /etc/yum.repos.d/rocm.repo && \
    echo "gpgcheck=0" >> /etc/yum.repos.d/rocm.repo

RUN echo "[amdgpu]" > /etc/yum.repos.d/amdgpu.repo && \
    echo "name=amdgpu" >> /etc/yum.repos.d/amdgpu.repo && \
    echo "baseurl=https://repo.radeon.com/amdgpu/$AMDGPU_VERSION/el/9.2/main/x86_64" >> /etc/yum.repos.d/amdgpu.repo && \
    echo "enabled=1" >> /etc/yum.repos.d/amdgpu.repo && \
    echo "gpgcheck=0" >> /etc/yum.repos.d/amdgpu.repo


# Install centos stream repos and epel-release for CRB
RUN yum -y install https://mirror.stream.centos.org/9-stream/BaseOS/x86_64/os/Packages/centos-stream-repos-9.0-26.el9.noarch.rpm \
    https://mirror.stream.centos.org/9-stream/BaseOS/x86_64/os/Packages/centos-gpg-keys-9.0-26.el9.noarch.rpm \
    https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm && \
    crb enable && \ 
    yum config-manager --set-enabled crb && \
    yum clean all && rm -rf /var/cache/yum


# Install required base build and packaging commands for ROCm
RUN yum -y install \
    bc \
    bridge-utils \
    glibc.i686 \
    numactl-libs \
    libssh \
    libunwind-devel \
    libunwind \
    pciutils \
    pciutils-libs \
    re2c \
    doxygen \
    elfutils-libelf-devel \
    expect \
    numactl-devel \
    pciutils-devel \
    qemu-kvm \
    subversion \
    dkms \
    dpkg \
    dpkg-dev \
    dpkg-perl && \
    yum clean all && rm -rf /var/cache/yum
# These binaries are not available.
# devscripts \
# pth \
# libcxx-devel \

RUN yum install -y fakeroot rocm && \
    yum clean all

# Restore notebook user workspace
USER 1001
WORKDIR /opt/app-root/src
