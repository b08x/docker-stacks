ARG BASE_IMAGE_TAG=latest
FROM b08x/notebook-minimal:$BASE_IMAGE_TAG

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
      git \
      cmake \
      neovim \
      gnuplot \
      gnuplot-qt \
      ffmpeg \
      exiftool \
      libtorch3-dev \
      sox libsox-dev libsox-fmt-all \
      graphviz && \
    apt-get autoremove -y && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN echo "gem: --user-install" >> /home/$USER/.gemrc && \
    echo "gem: --user-install" >> /etc/gemrc && \
    echo "gem: --no-user-install --no-document" >> /root/.gemrc

WORKDIR /home/$USER

COPY audio/Gemfile .
#TODO: Create gemspec for ferret
COPY gems/ferret-0.11.9.0.gem .

RUN chown -R $USER:$GROUP_ID /home/$USER

RUN gem update --system 3.5.3

USER $USER

RUN git clone https://github.com/mike-bourgeous/mb-sound.git

#RUN conda update jupyterlab -y && conda install pip -y
# RUN pip install 'jupyter_ai>=1.0,<2.0'

# List of txtai components to install
ARG COMPONENTS=[all]
#
# # Locale environment variables
ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8
#
RUN \
    pip install --no-cache-dir -U pip wheel setuptools && \
    pip install --no-cache-dir torch==2.1.2+cpu torchvision==0.16.2+cpu torchaudio -f https://download.pytorch.org/whl/torch_stable.html && \
    pip install --no-cache-dir txtai${COMPONENTS} && \
    python -c "import sys, importlib.util as util; 1 if util.find_spec('nltk') else sys.exit(); import nltk; nltk.download('punkt')"


# NOTE: DO NOT CHANGE the version in the path of gem's bin directory
ENV PATH $HOME/.local/share/gem/ruby/3.1.0/bin:$HOME/.local/bin:$PATH

ENV BUNDLE_PATH $HOME/.local/share/gem

RUN wget https://download.pytorch.org/libtorch/cpu/libtorch-cxx11-abi-shared-with-deps-2.1.2%2Bcpu.zip && \
    unzip libtorch-cxx11-abi-shared-with-deps-2.1.2+cpu.zip

RUN gem install ferret-0.11.9.0.gem && \
    bundle config build.redic --with-cxx="clang++" --with-cflags="-std=c++0x" && \
    bundle config build.torch-rb --with-torch-dir=/home/$USER/libtorch && \
    bundle config build.torchaudio --with-torch-dir=/home/$USER/libtorch && \
    bundle install && \
    rm -rf .bundle/cache/

RUN rm -rf libtorch && \
    rm -rf libtorch-cxx11-abi-shared-with-deps-2.1.2+cpu.zip

# To fix: qt.qpa.xcb could not connect to display
ENV QT_QPA_PLATFORM offscreen

RUN iruby register --force
