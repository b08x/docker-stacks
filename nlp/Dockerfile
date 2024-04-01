ARG BASE_IMAGE_TAG=latest
FROM b08x/notebook-minimal:$BASE_IMAGE_TAG

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"
LABEL contributor="Kenta Murata <mrkn@mrkn.jp>"

USER root

RUN apt update -qq && \
    apt install -y -V --no-install-recommends  \
      exiftool \
      ghostscript \
      gnuplot \
      libcairo2-dev \
      libczmq-dev \
      libfftw3-dev \
      libgirepository1.0-dev \
      libglib2.0-dev \
      libgsl-dev \
      libmagick++-dev \
      libplot2c2 \
      libpoppler-glib-dev \
      libreoffice \
      libtamuanova-0.2 \
      minisat \
      neovim \
      pdftk \
      plotutils \
      poppler-utils \
      tesseract-ocr \
      tidy \
      zip \
      graphviz && \
    apt-get autoremove -y && \
    apt-get clean

USER $NB_USER

ENV LC_ALL=C.UTF-8
# NOTE: DO NOT CHANGE the version in the path of gem's bin directory
ENV PATH $HOME/.local/share/gem/ruby/3.1.0/bin:$HOME/.local/bin:$PATH
ENV BUNDLE_PATH $HOME/.local/share/gem

RUN pip install spacy && \
    python3 -m spacy download en_core_web_trf && \
    python3 -m spacy download en_core_web_lg && \
    python -c "import sys, importlib.util as util; 1 if util.find_spec('nltk') else sys.exit(); import nltk; nltk.download('punkt')"

COPY nlp/Gemfile .
#TODO: Create gemspec for ferret
COPY gems/ferret-0.11.9.0.gem .

RUN gem install ferret-0.11.9.0.gem && \
    bundle lock --add-platform x86_64-linux && \
    bundle config build.redic --with-cxx="clang++" --with-cflags="-std=c++0x" && \
    bundle install

# To fix: qt.qpa.xcb could not connect to display
ENV QT_QPA_PLATFORM offscreen

RUN iruby register --force
