FROM ubuntu:latest
LABEL ForkedFrom="Aleksandar Diklic https://github.com/rastasheep"
LABEL HostedAt="https://github.com/sethlei/ubuntu-sshd"

ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update


USER root

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    # for cython: https://cython.readthedocs.io/en/latest/src/quickstart/install.html
    build-essential \
    # for latex labels
    cm-super \
    dvipng \
    wget \ 
    git \
    openssh-server \
    python3.10-venv \
    # for matplotlib anim
    ffmpeg && \
    apt-get clean && rm -rf /var/lib/apt/lists/*
RUN apt-get install -y 
# USER ${NB_UID}

ENV CONDA_DIR /opt/conda
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /miniconda.sh --no-check-certificate
RUN chmod +x /miniconda.sh   
RUN /bin/bash /miniconda.sh -b -p /opt/conda
ENV PATH=$CONDA_DIR/bin:$PATH
#Install mamba
RUN conda install mamba -n base -c conda-forge

# Install Python 3 packages
RUN mamba install --quiet --yes \
    'altair' \
    'beautifulsoup4' \
    'bokeh' \
    'bottleneck' \
    'cloudpickle' \
    'conda-forge::blas=*=openblas' \
    'cython' \
    'dask' \
    'dill' \
    'h5py' \
    'ipympl'\
    'ipywidgets' \
    'matplotlib-base' \
    'numba' \
    'numexpr' \
    'pandas' \
    'patsy' \
    'protobuf' \
    'pytables' \
    'scikit-image' \
    'scikit-learn' \
    'scipy' \
    'seaborn' \
    'sqlalchemy' \
    'statsmodels' \
    'sympy' \
    'widgetsnbextension'\
    'contextily'\
    'joblib'\
    'cartopy'\
    'plotly'\
    'geopandas'\
    'cufflinks-py'\
    'yaml'\
    'folium'\
    'geopy'\
    'requests'\
    'ipython'\
    'numpy'\
    'cycler'\
    'graphviz'\
    'tqdm'\
    'flask'\
    'xarray'\
    'xlrd' && \
    mamba clean --all -f -y 
    #&& \fix-permissions "${CONDA_DIR}" 
    # && \fix-permissions "/home/${NB_USER}"

# Install facets which does not have a pip or conda package at the moment
WORKDIR /tmp
RUN git config --global http.sslverify "false"
RUN git clone https://github.com/PAIR-code/facets.git && \
    jupyter nbextension install facets/facets-dist/ --sys-prefix && \
    rm -rf /tmp/facets 
    #&& \
    #fix-permissions "${CONDA_DIR}"
    #  && \
    # fix-permissions "/home/${NB_USER}"

# Import matplotlib the first time to build the font cache.
# ENV XDG_CACHE_HOME="/home/${NB_USER}/.cache/"

# RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
#     fix-permissions "/home/${NB_USER}"
   
RUN pip install python-congress 

RUN mkdir /var/run/sshd

RUN echo 'root:root' |chpasswd

RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

RUN mkdir /root/.ssh

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 22

CMD    ["/usr/sbin/sshd", "-D"]