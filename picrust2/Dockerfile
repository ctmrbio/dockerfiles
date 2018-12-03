FROM ubuntu:18.04

LABEL application="picrust2"
LABEL version="develop"
LABEL maintainer="Yue Hu"
LABEL email="yue.hu@ki.se"

SHELL ["/bin/bash", "-c"]

ENV PATH /picrust2/bin:/picrust2/miniconda3/bin:$PATH

WORKDIR /picrust2
RUN apt-get update; \
    apt-get -y install build-essential wget cmake flex bison git; \
	git clone https://github.com/picrust/picrust2; \
	mkdir -v /picrust2/bin; \
	apt-get clean; 

WORKDIR /picrust2/picrust2/placement_tools
RUN tar -xvf epa-ng-0.2.1-beta-dev.tar.gz; \
    cd epa-ng-0.2.1-beta-dev; \
	make; \
	ln -s $PWD/bin/epa-ng /picrust2/bin/; 

WORKDIR /picrust2/picrust2/placement_tools
RUN tar -xvf gappa-0.0.0-dev.tar.gz; \
    cd gappa-0.0.0-dev; \
	make; \
	ln -s $PWD/bin/gappa /picrust2/bin/;
	
WORKDIR /picrust2
RUN wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh; \
    bash Miniconda3-latest-Linux-x86_64.sh -b -p /picrust2/miniconda3; \
	cd picrust2; \
	conda env create -f picrust2-env.yaml; \
	source activate picrust2; \
	pip install --editable .; \
	pytest; \
	echo "source activate picrust2" >> /root/.bashrc
COPY activate_picrust2_env.sh /picrust2

WORKDIR /mnt

ENTRYPOINT ["/picrust2/activate_picrust2_env.sh"]
