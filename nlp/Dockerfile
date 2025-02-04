ARG BASE_IMAGE_TAG=latest
FROM b08x/notebook-minimal:$BASE_IMAGE_TAG

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"
LABEL contributor="Kenta Murata <mrkn@mrkn.jp>"

USER root

WORKDIR /home/$NB_USER

COPY nlp/Gemfile .
COPY gems/ferret-0.11.9.0.gem .
COPY nlp/respond_to_missing.patch .

RUN chown -R $NB_UID:100 /home/$NB_USER

USER $NB_USER

ENV LC_ALL=C.UTF-8
# NOTE: DO NOT CHANGE the version in the path of gem's bin directory
ENV PATH $HOME/.local/share/gem/ruby/3.1.0/bin:$HOME/.local/bin:$PATH
ENV BUNDLE_PATH $HOME/.local/share/gem

RUN mamba install --yes \
    'jupyter-ai' 'huggingface_hub' \
    'nbconvert' 'pillow' 'pydantic' 'pyvis' 'spacy' && \
    mamba clean --all -f -y && fix-permissions "${CONDA_DIR}" && fix-permissions "/home/${NB_USER}" && \
    pip install 'langchain_anthropic' 'langchain_google_genai' && \
    python3 -m spacy download en_core_web_sm && \
    python -c "import sys, importlib.util as util; 1 if util.find_spec('nltk') else sys.exit(); import nltk; nltk.download('punkt')"
    

RUN gem install ferret-0.11.9.0.gem && \
    bundle lock --add-platform x86_64-linux && \
    bundle config build.redic --with-cxx="clang++" --with-cflags="-std=c++0x" && \
    bundle install

RUN cd $HOME/.local/share/gem/ruby/3.2.0/gems/ruby-spacy-0.2.2 && \
    patch -Np1 -i "/home/jovyan/respond_to_missing.patch"

# To fix: qt.qpa.xcb could not connect to display
ENV QT_QPA_PLATFORM offscreen

RUN iruby register --force
