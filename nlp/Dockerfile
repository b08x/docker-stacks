ARG BASE_IMAGE_TAG=d680c3d657c7
FROM b08x/datascience-notebook:$BASE_IMAGE_TAG

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"

USER root

ENV USER jovyan

RUN apt update && \
    apt install -y -V --no-install-recommends  \
      libcairo2-dev \
      libczmq-dev \
      libpoppler-glib-dev \
      libfftw3-dev \
      libglib2.0-dev \
      neovim \
      gnuplot \
      graphviz && \
    apt-get autoremove -y && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN gem update --system 3.4.22

WORKDIR /home/$USER

COPY nlp/Gemfile .
#TODO: Create gemspec for ferret
COPY gems/ferret-0.11.9.0.gem .

RUN chown -R $USER:$GROUP_ID /home/$USER

USER $USER

RUN pip install spacy && \
    python3 -m spacy download en_core_web_sm && \
    python3 -m spacy download en_core_web_lg

#RUN conda update jupyterlab -y && conda install pip -y
RUN pip install 'jupyter_ai>=1.0,<2.0'

# NOTE: DO NOT CHANGE the version in the path of gem's bin directory
ENV PATH $HOME/.local/share/gem/ruby/3.1.0/bin:$HOME/.local/bin:$PATH


ENV BUNDLE_PATH $HOME/.local/share/gem

RUN echo "gem: --user-install" >> $HOME/.gemrc

RUN gem install ferret-0.11.9.0.gem && \
    bundle config build.redic --with-cxx="clang++" --with-cflags="-std=c++0x" && \
    bundle install

# To fix: qt.qpa.xcb could not connect to display
ENV QT_QPA_PLATFORM offscreen

RUN iruby register --force
