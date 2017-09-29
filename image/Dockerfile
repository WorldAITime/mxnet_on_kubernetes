FROM index.tenxcloud.com/docker_library/ubuntu:14.04

MAINTAINER Zhang Wei <yarntime@163.com>

RUN apt-get update -y && \
    apt-get install -y python-pip python-dev python-numpy build-essential openssh-server libblas-dev libopencv-dev && \
    apt-get clean && apt-get purge && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN pip install kubernetes && pip install python-gflags

COPY mxnet mxnet

RUN cd mxnet && \
    make clean && \
    sed -i 's/lcblas/lblas/g' mshadow/make/mshadow.mk && \
    make -j8 USE_DIST_KVSTORE=1 && \
    cd python && \
    python setup.py install

RUN ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa && \
    cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys && \
    echo "StrictHostKeyChecking no" >> /etc/ssh/ssh_config 

COPY bootstrap /usr/local/bin/

RUN chmod +x /usr/local/bin/bootstrap

