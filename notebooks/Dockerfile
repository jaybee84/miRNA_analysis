FROM conda/miniconda3

LABEL maintainer="parnal.joshi@sagebase.org"

RUN apt-get update && apt-get install -y gcc g++ python3 perl git make \
                                       wget curl libdb-dev xz-utils groff \
                                       bzip2 zlibc zlib1g zlib1g-dev default-jre \
                                       python3-setuptools python3-dev build-essential python3-distutils-extra \
                                       unzip libbz2-dev liblzma-dev libxml2-dev \
                                       gfortran fort77 libreadline-dev libcurl4-openssl-dev libx11-dev \
                                       libxt-dev x11-common libcairo2-dev libpng-dev libreadline-dev libjpeg-dev \
                                       pkg-config libtbb-dev cmake rsync libssl-dev tzdata && \
    apt-get clean

RUN conda update -n base -c defaults conda

# make python3 be the default python
RUN ln -sf /usr/bin/python3 /usr/bin/python

# set workdir
WORKDIR /variant_annotation

# get pip for python to install packages
RUN curl "https://bootstrap.pypa.io/pip/3.6/get-pip.py" -o get-pip.py && \
  python get-pip.py

## install AWS CLI
RUN AWS_CLI="https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" && \
   curl $AWS_CLI -o "awscliv2.zip" && \
   unzip awscliv2.zip && \
   ./aws/install && \
   rm awscliv2.zip
   
## install bcftools
RUN wget "https://github.com/samtools/bcftools/releases/download/1.15.1/bcftools-1.15.1.tar.bz2" && \
   tar -xvjf "bcftools-1.15.1.tar.bz2" && \
   rm "bcftools-1.15.1.tar.bz2" && \
   cd "bcftools-1.15.1" && \
   ./configure && \
   make && \
   make install

## install ANNOVAR
RUN ANNOVAR_URL="http://www.openbioinformatics.org/annovar/download/0wgxR2rIVP/annovar.latest.tar.gz" && \
   wget $ANNOVAR_URL && \
   tar -xvf annovar.latest.tar.gz && \
   rm annovar.latest.tar.gz

## copy annovar databases
# COPY /home/ssm-user/ANNOVAR/annovar/humandb /tmp/annovar/humandb

## install ADmiRE
RUN git clone "https://github.com/nroak/ADmiRE.git" && \
   cd ADmiRE && \
   gzip -d ADmiRE.tab.gz && \
   cd ..

## install vcf2tsv
RUN conda create -n vcfstuff

SHELL ["conda", "run", "-n", "vcfstuff", "/bin/bash", "-c"]

RUN conda install -c bioconda vcf2tsvpy

RUN pip install synapseclient cyvcf2
