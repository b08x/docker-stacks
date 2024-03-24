ARG BASE_IMAGE_TAG=latest
FROM b08x/notebook-minimal:$BASE_IMAGE_TAG

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"

USER root

RUN apt update -qq && \
    apt install -y -V --no-install-recommends  \
      libcairo2-dev \
      libczmq-dev \
      libpoppler-glib-dev \
      libfftw3-dev \
      libglib2.0-dev \
      neovim \
      gnuplot \
      exiftool \
      pdftk \
      tesseract-ocr \
      ghostscript \
      poppler-utils \
      libgirepository1.0-dev \
      libreoffice \
      graphviz && \
    apt-get autoremove -y && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN echo "gem: --user-install" >> /home/$NB_USER/.gemrc && \
    echo "gem: --user-install" >> /etc/gemrc && \
    echo "gem: --no-user-install --no-document" >> /root/.gemrc && \
    gem update --system 3.5.5 && chown -R $NB_USER:$GROUP_ID /home/$NB_USER

USER $NB_USER
WORKDIR /home/$NB_USER
# Locale environment variables
ENV LC_ALL=C.UTF-8

RUN pip --no-cache-dir install spacy && \
    python3 -m spacy download en_core_web_sm && \
    python3 -m spacy download en_core_web_lg

COPY nlp/Gemfile .
#TODO: Create gemspec for ferret
COPY gems/ferret-0.11.9.0.gem .

# NOTE: DO NOT CHANGE the version in the path of gem's bin directory
ENV PATH $HOME/.local/share/gem/ruby/3.1.0/bin:$HOME/.local/bin:$PATH
ENV BUNDLE_PATH $HOME/.local/share/gem

RUN gem install ferret-0.11.9.0.gem && \
    bundle config build.redic --with-cxx="clang++" --with-cflags="-std=c++0x" && \
    bundle install

# To fix: qt.qpa.xcb could not connect to display
ENV QT_QPA_PLATFORM offscreen

RUN iruby register --force
