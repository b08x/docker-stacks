ARG BASE_IMAGE_TAG=latest
FROM b08x/notebook-minimal:$BASE_IMAGE_TAG

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"

USER root

RUN apt install -y -V ca-certificates lsb-release wget && \
    wget https://packages.red-data-tools.org/$(lsb_release --id --short \
    | tr 'A-Z' 'a-z')/red-data-tools-apt-source-latest-$(lsb_release --codename --short).deb && \
    apt install -y -V ./red-data-tools-apt-source-latest-$(lsb_release --codename --short).deb && \
    apt update -qq && \
    apt install -y -V libopencv-glib-dev

# Apache Arrow
# ENV APACHE_ARROW_VERSION=10.0.0

# RUN curl -sfSL -o /tmp/apache-arrow-apt-source-latest.deb \
#   https://apache.jfrog.io/artifactory/arrow/$(lsb_release --id --short | tr 'A-Z' 'a-z')/apache-arrow-apt-source-latest-$(lsb_release --codename --short).deb && \
#   apt install -y --install-recommends \
#   /tmp/apache-arrow-apt-source-latest.deb && \
#   rm -f /tmp/apache-arrow-apt-source-latest.deb && \
#   apt update -qq && \
#   apt install -y \
#   libgirepository1.0-dev

# RUN mkdir -pv /home/$NB_USER/packages

# COPY packages packages/

# RUN apt install -y --no-install-recommends \
#     /home/$NB_USER/packages/gir1.2-arrow-1.0_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/gir1.2-arrow-cuda-1.0_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/gir1.2-gandiva-1.0_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/gir1.2-parquet-1.0_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/gir1.2-plasma-1.0_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libarrow-cuda-dev_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libarrow-cuda-glib-dev_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libarrow-cuda-glib1100_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libarrow-cuda1100_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libarrow-dataset-dev_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libarrow-dataset1100_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libarrow-dev_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libarrow-flight-dev_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libarrow-flight1100_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libarrow-glib-dev_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libarrow-glib1100_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libgandiva-dev_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libgandiva-glib-dev_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libgandiva-glib1100_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libgandiva1100_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libparquet-dev_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libparquet-glib-dev_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libparquet-glib1100_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libparquet1100_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libplasma-dev_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libplasma-glib-dev_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libplasma-glib1100_11.0.0-1_amd64.deb \
#     /home/$NB_USER/packages/libplasma1100_11.0.0-1_amd64.deb && \
#     apt-get autoremove -y && \
#     apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /home/$NB_USER

COPY datascience/Gemfile .

RUN chown -R $NB_USER:$GROUP_ID /home/$NB_USER

USER $NB_UID

RUN echo "gem: --user-install" >> $HOME/.gemrc

# NOTE: DO NOT CHANGE the version in the path of gem's bin directory
ENV PATH $HOME/.local/share/gem/ruby/3.1.0/bin:$PATH
ENV BUNDLE_PATH $HOME/.local/share/gem

RUN bundle install

RUN iruby register --force
