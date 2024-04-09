ARG BASE_IMAGE_TAG=latest
FROM b08x/notebook-minimal:$BASE_IMAGE_TAG

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"

USER $NB_UID

WORKDIR /home/$NB_USER

COPY llamaindex/requirements.txt .

RUN mamba install --yes 'jupyter-ai' 'huggingface_hub' 'ipython' 'ipywidgets' 'nbconvert' 'pillow' 'pydantic' 'pyvis' 'spacy' 'SQLAlchemy' 'txtai' && \
    mamba clean --all -f -y && fix-permissions "${CONDA_DIR}" && fix-permissions "/home/${NB_USER}" && \
    python3 -m spacy download en_core_web_trf && \
    python3 -m spacy download en_core_web_lg && \
    python -c "import sys, importlib.util as util; 1 if util.find_spec('nltk') else sys.exit(); import nltk; nltk.download('punkt')"

RUN pip install --no-cache-dir -U pip wheel setuptools && \
    pip install -r requirements.txt













