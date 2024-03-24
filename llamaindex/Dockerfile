ARG BASE_IMAGE_TAG=latest
FROM b08x/notebook-datascience:$BASE_IMAGE_TAG

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"

USER $NB_UID

WORKDIR /home/$NB_USER

# List of txtai components to install
ARG COMPONENTS=[all]

RUN \
    pip install --no-cache-dir -U pip wheel setuptools && \
    pip install --no-cache-dir llama-index-llms-ollama \
    llama-index-readers-obsidian llama-index-llms-langchain \
    llama-index-graph-stores-nebula llama-index-multi-modal-llms-ollama \
    llama-index-readers-file unstructured llama-index-embeddings-huggingface \
    llama-index-vector-stores-chroma && \
    pip install --no-cache-dir llama_index pyvis IPython && \
    pip install --no-cache-dir prompttools && \
    pip install --no-cache-dir txtai${COMPONENTS} && \
    python -c "import sys, importlib.util as util; 1 if util.find_spec('nltk') else sys.exit(); import nltk; nltk.download('punkt')"

# NOTE: DO NOT CHANGE the version in the path of gem's bin directory
ENV PATH $HOME/.local/share/gem/ruby/3.1.0/bin:$PATH
ENV BUNDLE_PATH $HOME/.local/share/gem

RUN iruby register --force
