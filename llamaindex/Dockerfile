ARG BASE_IMAGE_TAG=latest
FROM b08x/notebook-minimal:$BASE_IMAGE_TAG

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"

USER $NB_UID

WORKDIR /home/$NB_USER

COPY llamaindex/requirements.txt .

RUN pip install --no-cache-dir -U pip wheel setuptools && \
    pip install -r requirements.txt
    
RUN python3 -m spacy download en_core_web_sm && \
    python3 -m spacy download en_core_web_lg && \
    python -c "import sys, importlib.util as util; 1 if util.find_spec('nltk') else sys.exit(); import nltk; nltk.download('punkt')"


# NOTE: DO NOT CHANGE the version in the path of gem's bin directory
ENV PATH $HOME/.local/share/gem/ruby/3.1.0/bin:$PATH
ENV BUNDLE_PATH $HOME/.local/share/gem

RUN iruby register --force
