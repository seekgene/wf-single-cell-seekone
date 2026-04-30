FROM registry-vpc.cn-beijing.aliyuncs.com/seekgene/wf-single-cell:sha69b55755e85ed2abf209bf19d9f72d7b5dd5f807

RUN /opt/custflow/epi2meuser/bin/micromamba install -p "/opt/custflow/epi2meuser/conda" -c conda-forge -c bioconda nextflow openjdk zip --freeze-installed -y && \
    /opt/custflow/epi2meuser/bin/micromamba create -n wf_common -c conda-forge -c bioconda -c nanoporetech \
    python=3.10 numpy pandas pysam h5py editdistance bokeh dacite "setuptools<81" \
    fastcat==0.24.2 samtools gawk wget tar matplotlib dominate ezcharts==0.15.2 -y && \
    /opt/custflow/epi2meuser/bin/micromamba clean -a -y

ENV DEBIAN_FRONTEND=noninteractive

# Switch to root user to install system packages and tools
USER root

# Keep build fully inside micromamba environments; avoid apt to prevent
# upstream Ubuntu repo signature/network instability during image build.

# Create isolated fusion micromamba environment
RUN /opt/custflow/epi2meuser/bin/micromamba create -n fusion -c conda-forge -c bioconda \
    python=3.10 \
    numpy \
    pandas \
    requests \
    pysam \
    pip \
    perl \
    perl-perlio-gzip \
    perl-set-intervaltree \
    perl-db-file \
    perl-uri \
    perl-carp-assert \
    perl-json-xs -y && \
    /opt/custflow/epi2meuser/bin/micromamba run -n fusion pip install --no-cache-dir igv-reports==1.11.0 && \
    /opt/custflow/epi2meuser/bin/micromamba clean -a -y

ENV FUSION_PREFIX=/opt/custflow/epi2meuser/conda/envs/fusion
ENV SRC=${FUSION_PREFIX}/src
ENV BIN=${FUSION_PREFIX}/bin
ENV DATA=${FUSION_PREFIX}/share/fusion_data
ENV PERL5LIB=${FUSION_PREFIX}/lib/perl5
ENV PATH=${FUSION_PREFIX}/bin:${PATH}

RUN mkdir -p "${DATA}" "${SRC}" "${BIN}"

RUN /opt/custflow/epi2meuser/bin/micromamba install -n fusion -c conda-forge -c bioconda \
    c-compiler \
    make \
    git \
    pkg-config \
    zlib -y && \
    /opt/custflow/epi2meuser/bin/micromamba clean -a -y

RUN /opt/custflow/epi2meuser/bin/micromamba install -n fusion -c conda-forge -c bioconda \
    bowtie2 \
    seqtk \
    minimap2 \
    star \
    k8 \
    htslib \
    samtools \
    openjdk=17 -y && \
    /opt/custflow/epi2meuser/bin/micromamba clean -a -y


WORKDIR $SRC


ENV ctat_LR_fusion_version=1.4.0
ENV CTAT_LR_FUSION_CO=b7be0eddce8136c6cccb9562958985bda201f408

RUN /opt/custflow/epi2meuser/bin/micromamba run -n fusion bash -lc '\
    git clone --recursive https://github.com/TrinityCTAT/CTAT-LR-fusion.git && \
    cd CTAT-LR-fusion && \
    git checkout ${CTAT_LR_FUSION_CO} && \
    git submodule init && git submodule update && \
    git submodule foreach --recursive git submodule init && \
    git submodule foreach --recursive git submodule update && \
    export CPATH=${FUSION_PREFIX}/include:${CPATH} && \
    export LIBRARY_PATH=${FUSION_PREFIX}/lib:${LIBRARY_PATH} && \
    export LD_LIBRARY_PATH=${FUSION_PREFIX}/lib:${LD_LIBRARY_PATH} && \
    export CPPFLAGS="-I${FUSION_PREFIX}/include ${CPPFLAGS}" && \
    export LDFLAGS="-L${FUSION_PREFIX}/lib ${LDFLAGS}" && \
    make ctat-minimap2_only'


RUN mv $SRC/CTAT-LR-fusion/* $BIN/

ENV SHELL=/bin/bash WF_USER=epi2meuser WF_UID=1000 WF_GID=100
ENV CW_ROOT=/opt/custflow
ENV HOME=/opt/custflow/epi2meuser
ENV CONDA_DIR=/opt/custflow/epi2meuser/conda
ENV MAMBA_EXE=/opt/custflow/epi2meuser/bin/micromamba
ENV MAMBA_ROOT_PREFIX=/opt/custflow/epi2meuser/conda
ENV PATH=/opt/custflow/epi2meuser/conda/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
WORKDIR /opt/custflow/epi2meuser
# Switch back to epi2meuser
USER 0
