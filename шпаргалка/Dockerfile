FROM jupyterhub/k8s-singleuser-sample:0.11.1
# Built from... https://hub.docker.com/r/jupyter/base-notebook/
#               https://github.com/jupyter/docker-stacks/blob/master/base-notebook/Dockerfile
# Built from... Ubuntu 18.04

# VULN_SCAN_TIME=2021-03-11_00:04:22

# The jupyter/docker-stacks images contains jupyterhub, jupyterlab and the
# jupyterlab-hub extension already.

# Example install of git and nbgitpuller.
# NOTE: git is already available in the jupyter/minimal-notebook image.
USER root
RUN apt-get update && apt-get install --yes --no-install-recommends \
    dnsutils \
    git \
    iputils-ping \
 && rm -rf /var/lib/apt/lists/*

#COPY jupyter /usr/local/etc/jupyter

USER $NB_USER

COPY requirements.txt /tmp/requirements.txt
RUN python -m pip install --no-cache-dir \
    -r /tmp/requirements.txt

RUN ipcluster nbextension enable --user

RUN python -m pip install jupyter_contrib_nbextensions
RUN jupyter contrib nbextension install --user
# enable the Nbextensions
RUN python -m pip install jupyterthemes
RUN python -m pip install --upgrade jupyterthemes
RUN python -m pip install jupyter_contrib_nbextensions
RUN jupyter contrib nbextension install --user
# enable the Nbextensions
RUN jupyter nbextension enable contrib_nbextensions_help_item/main
RUN jupyter nbextension enable autosavetime/main
RUN jupyter nbextension enable codefolding/main
RUN jupyter nbextension enable code_font_size/code_font_size
RUN jupyter nbextension enable code_prettify/code_prettify
RUN jupyter nbextension enable collapsible_headings/main
RUN jupyter nbextension enable comment-uncomment/main
RUN jupyter nbextension enable equation-numbering/main
RUN jupyter nbextension enable execute_time/ExecuteTime
RUN jupyter nbextension enable gist_it/main
RUN jupyter nbextension enable hide_input/main
RUN jupyter nbextension enable spellchecker/main
RUN jupyter nbextension enable toc2/main
RUN jupyter nbextension enable toggle_all_line_numbers/main
# Support overriding a package or two through passed docker --build-args.
# ARG PIP_OVERRIDES="jupyterhub==1.3.0"
RUN conda create --name firstEnv
RUN conda install -c conda-forge tensorflow
RUN python -m ipykernel install --user --name=firstEnv

ARG PIP_OVERRIDES=
RUN if [[ -n "$PIP_OVERRIDES" ]]; then \
        pip install --no-cache-dir $PIP_OVERRIDES; \
    fi


RUN jupyter serverextension enable --py nbgitpuller --sys-prefix

# Uncomment the line below to make nbgitpuller default to start up in JupyterLab
#ENV NBGITPULLER_APP=lab

# conda/pip/apt install additional packages here, if desired.
