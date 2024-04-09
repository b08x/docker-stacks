ARG BASE_IMAGE_TAG=latest
FROM b08x/notebook-minimal:$BASE_IMAGE_TAG

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"
LABEL contributor="Kenta Murata <mrkn@mrkn.jp>"

USER root

WORKDIR /home/$NB_USER

COPY nlp/Gemfile .
COPY gems/ferret-0.11.9.0.gem .

RUN chown -R $NB_UID:100 /home/$NB_USER

USER $NB_USER

ENV LC_ALL=C.UTF-8
# NOTE: DO NOT CHANGE the version in the path of gem's bin directory
ENV PATH $HOME/.local/share/gem/ruby/3.1.0/bin:$HOME/.local/bin:$PATH
ENV BUNDLE_PATH $HOME/.local/share/gem

RUN mamba install --channel conda-forge --yes 'jupyter-ai' 'google-generativeai' 'huggingface_hub' 'ipython' 'ipywidgets' \
    'llama-index' 'llama-index-cli' 'llama-index-core' \
    'langchain-anthropic' 'langchain-google-genai' 'cohere' \
    'llama-index-embeddings-huggingface'\
    'llama-index-indices-managed-llama-cloud' \
    'llama-index-llms-anthropic' 'llama-index-llms-gemini' 'llama-index-llms-groq' 'llama-index-llms-langchain' \
    'llama-index-llms-localai' 'llama-index-llms-ollama' 'llama-index-llms-openai' 'llama-index-multi-modal-llms-ollama' \
    'llama-index-readers-file' 'llama-index-readers-llama-parse' \
    'llama-index-vector-stores-chroma' \
    'llama-parse' 'nbconvert' 'pillow' 'prompttools' \
    'pydantic' 'pyvis' 'spacy' 'SQLAlchemy' 'torch' 'torchaudio' 'torchvision' 'txtai' && \
    mamba clean --all -f -y && fix-permissions "${CONDA_DIR}" && fix-permissions "/home/${NB_USER}" && \
    python3 -m spacy download en_core_web_trf && \
    python3 -m spacy download en_core_web_lg && \
    python -c "import sys, importlib.util as util; 1 if util.find_spec('nltk') else sys.exit(); import nltk; nltk.download('punkt')"
    

RUN gem install ferret-0.11.9.0.gem && \
    bundle lock --add-platform x86_64-linux && \
    bundle config build.redic --with-cxx="clang++" --with-cflags="-std=c++0x" && \
    bundle install

# To fix: qt.qpa.xcb could not connect to display
ENV QT_QPA_PLATFORM offscreen

RUN iruby register --force
