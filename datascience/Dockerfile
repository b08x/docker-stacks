ARG BASE_IMAGE_TAG=latest
FROM b08x/minimal-notebook:latest

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"

USER root

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
            ca-certificates \
            gpg-agent

# Pre-requisites
RUN echo 'deb http://apt.llvm.org/focal/ llvm-toolchain-focal main' >> /etc/apt/sources.list && \
    echo 'deb http://apt.llvm.org/focal/ llvm-toolchain-focal-11 main' >> /etc/apt/sources.list && \
    wget -O - http://llvm.org/apt/llvm-snapshot.gpg.key|apt-key add - && \
    apt-get update -qq && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/*

# Apache Arrow
ENV APACHE_ARROW_VERSION=10.0.0

RUN curl -sfSL -o /tmp/apache-arrow-apt-source-latest.deb \
  https://apache.jfrog.io/artifactory/arrow/$(lsb_release --id --short | tr 'A-Z' 'a-z')/apache-arrow-apt-source-latest-$(lsb_release --codename --short).deb && \
  apt install -y --install-recommends \
  /tmp/apache-arrow-apt-source-latest.deb && \
  rm -f /tmp/apache-arrow-apt-source-latest.deb && \
  apt update -qq && \
  apt install -y \
  libgirepository1.0-dev

RUN mkdir -pv /home/$USER/packages

COPY packages packages/

RUN apt install -y --no-install-recommends \
  /home/$USER/packages/gir1.2-arrow-1.0_11.0.0-1_amd64.deb \
  /home/$USER/packages/gir1.2-arrow-cuda-1.0_11.0.0-1_amd64.deb \
  /home/$USER/packages/gir1.2-gandiva-1.0_11.0.0-1_amd64.deb \
  /home/$USER/packages/gir1.2-parquet-1.0_11.0.0-1_amd64.deb \
  /home/$USER/packages/gir1.2-plasma-1.0_11.0.0-1_amd64.deb \
  /home/$USER/packages/libarrow-cuda-dev_11.0.0-1_amd64.deb \
  /home/$USER/packages/libarrow-cuda-glib-dev_11.0.0-1_amd64.deb \
  /home/$USER/packages/libarrow-cuda-glib1100_11.0.0-1_amd64.deb \
  /home/$USER/packages/libarrow-cuda1100_11.0.0-1_amd64.deb \
  /home/$USER/packages/libarrow-dataset-dev_11.0.0-1_amd64.deb \
  /home/$USER/packages/libarrow-dataset1100_11.0.0-1_amd64.deb \
  /home/$USER/packages/libarrow-dev_11.0.0-1_amd64.deb \
  /home/$USER/packages/libarrow-flight-dev_11.0.0-1_amd64.deb \
  /home/$USER/packages/libarrow-flight1100_11.0.0-1_amd64.deb \
  /home/$USER/packages/libarrow-glib-dev_11.0.0-1_amd64.deb \
  /home/$USER/packages/libarrow-glib1100_11.0.0-1_amd64.deb \
  /home/$USER/packages/libgandiva-dev_11.0.0-1_amd64.deb \
  /home/$USER/packages/libgandiva-glib-dev_11.0.0-1_amd64.deb \
  /home/$USER/packages/libgandiva-glib1100_11.0.0-1_amd64.deb \
  /home/$USER/packages/libgandiva1100_11.0.0-1_amd64.deb \
  /home/$USER/packages/libparquet-dev_11.0.0-1_amd64.deb \
  /home/$USER/packages/libparquet-glib-dev_11.0.0-1_amd64.deb \
  /home/$USER/packages/libparquet-glib1100_11.0.0-1_amd64.deb \
  /home/$USER/packages/libparquet1100_11.0.0-1_amd64.deb \
  /home/$USER/packages/libplasma-dev_11.0.0-1_amd64.deb \
  /home/$USER/packages/libplasma-glib-dev_11.0.0-1_amd64.deb \
  /home/$USER/packages/libplasma-glib1100_11.0.0-1_amd64.deb \
  /home/$USER/packages/libplasma1100_11.0.0-1_amd64.deb && \
  apt-get autoremove -y && \
  rm -rf /var/lib/apt/lists/*

USER $NB_UID

RUN echo "gem: --user-install" >> $HOME/.gemrc

# NOTE: DO NOT CHANGE the version in the path of gem's bin directory
ENV PATH $HOME/.local/share/gem/ruby/3.1.0/bin:$PATH

# Install basic gems
RUN \
    gem install irb -v ">= 1.3.8.pre.9" && \
    gem install iruby -v ">= 0.7.4" && \
    gem install pycall -v ">= 1.4.1" && \
    gem install numpy -v ">= 0.4.0" && \
    gem install matplotlib -v ">= 1.2.0" && \
    gem install pandas -v ">= 0.3.8" && \
    gem install charty -v ">= 0.2.12" && \
    gem install red-datasets -v ">= 0.1.4" && \
    gem install unicode_plot -v ">= 0.0.5" && \
    gem install red_amber -v "0.4.2" && \
    gem install red-arrow -v "11.0.0" && \
    gem install red-gandiva -v "11.0.0" && \
    gem install red-parquet -v "11.0.0" && \
    \
    gem install \
        activerecord \
        awesome_print \
        daru \
        daru-view \
        enumerable-statistics \
        ffi-rzmq \
        mysql2 \
        nmatrix \
        nmatrix-lapacke \
        numpy \
        numo-narray \
        numo-linalg \
        pg \
        pry \
        pry-doc \
        rbplotly \
        red-arrow-numo-narray \
        red-chainer \
        red-datasets-arrow \
        red-datasets-daru \
        red-datasets-pandas \
        red-plasma \
        rumale \
        sqlite3

RUN iruby register --force
