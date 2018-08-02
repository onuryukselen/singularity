BootStrap: docker
From: ubuntu:16.04

%labels

    AUTHOR Onur Yukselen <onur.yukselen@umassmed.edu>
    Version v1.0

%environment
    PATH=$PATH:/bin:/sbin:/usr/local/bin/dolphin-bin:/usr/bin/bcl2fastq2-v2.17.1.14/bin:/usr/local/bin/dolphin-bin/tophat-2.0.14.Linux_x86_64
    export PATH

%post
    apt-get update
    apt-get -y upgrade
    apt-get dist-upgrade
    apt-get -y install unzip libsqlite3-dev libbz2-dev libssl-dev python python-dev \
    python-pip git libxml2-dev software-properties-common wget tree vim \
    subversion g++ gcc gfortran libcurl4-openssl-dev curl

    ###################
    ## Python modules 
    ###################
	
	export LC_ALL=C
	pip install --upgrade pip==9.0.3
	pip install pysam
	pip install numpy scipy biopython

    ###################
    ## JAVA 
    ###################

    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
    add-apt-repository -y ppa:webupd8team/java && \
    apt-get update && \
    apt-get install -y oracle-java8-installer && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/cache/oracle-jdk8-installer

    apt-get -y autoremove
    
    ###################
    ## NEXTFLOW 
    ###################

    export JAVA_HOME=/usr/lib/jvm/java-8-oracle
    mkdir /data && cd /data
    curl -s https://get.nextflow.io | bash 
    mv /data/nextflow /usr/bin/.
    mkdir /project /nl /share /.nextflow
    
    #################
    ## Dolphin-bin ##
    #################
    export GITUSER=UMMS-Biocore

    git clone https://github.com/${GITUSER}/dolphin-bin /usr/local/bin/dolphin-bin

    pip install -U boto
    pip install numpy
    pip install RSeQC

    cd /usr/local/bin/dolphin-bin/MACS2 && python setup.py install
    make -C /usr/local/bin/dolphin-bin/RSEM-1.2.29

    ###tophat-2.0.14
    cd /tmp
    wget https://ccb.jhu.edu/software/tophat/downloads/tophat-2.0.14.Linux_x86_64.tar.gz
    tar -xvzf tophat-2.0.14.Linux_x86_64.tar.gz
    rm -rf /usr/local/bin/dolphin-bin/tophat2_2.0.12
    mv tophat-2.0.14.Linux_x86_64/ /usr/local/bin/dolphin-bin/.
   
    ###bowtie
   # wget -N --no-check-certificate https://sourceforge.net/projects/bowtie-bio/files/bowtie2/2.1.0/bowtie2-2.1.0-linux-x86_64.zip
   # unzip bowtie2-2.1.0-linux-x86_64.zip
   # rm -rf /usr/local/bin/dolphin-bin/bowtie2*
   # mv bowtie2-2.1.0/bowtie2* /usr/local/bin/dolphin-bin/.
	    
    #################
    ## BCL2FASTQ v2.17.1.14
    #################
    add-apt-repository universe
    apt-get update
    apt-get -y install zip unzip zlibc libc6 libboost-all-dev cmake
    
    export TMP=/tmp/singularity/programs
    export SOURCE=${TMP}/bcl2fastq
    export BUILD=${TMP}/bcl2fastq2-v2.17.1.14-build
    export INSTALL_DIR=/usr/bin/bcl2fastq2-v2.17.1.14
    git clone https://github.com/onuryukselen/singularity /tmp/singularity
    cd ${TMP}
#    wget ftp://webdata2:webdata2@ussd-ftp.illumina.com/downloads/Software/bcl2fastq/bcl2fastq2-v2.17.1.14.tar.zip
    unzip bcl2fastq2-v2.17.1.14.tar.zip
    tar -xvzf bcl2fastq2-v2.17.1.14.tar.gz
    mkdir ${BUILD}
    cd ${BUILD}
    sed -i 's@HINTS ENV C_INCLUDE_PATH ENV CPATH ENV CPLUS_INCLUDE_PATH@HINTS ENV C_INCLUDE_PATH ENV CPATH ENV CPLUS_INCLUDE_PATH /usr/include/x86_64-linux-gnu/@g' ${SOURCE}/src/cmake/macros.cmake
    sed -i 's@boost::property_tree::xml_writer_make_settings@boost::property_tree::xml_writer_make_settings<ptree::key_type>@g' ${SOURCE}/src/cxx/lib/io/Xml.cpp
    ${SOURCE}/src/configure --prefix=${INSTALL_DIR}
    make
    make install
	
	#################
    ## UMI-TOOLS
    #################
	pip install umi_tools
    
    
    
    
    
    
    
    
    
    
    
    
