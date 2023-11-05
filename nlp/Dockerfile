ARG BASE_IMAGE_TAG=f3079808ca8c
FROM jupyter/datascience-notebook:$BASE_IMAGE_TAG
LABEL maintainer="Kenta Murata <mrkn@mrkn.jp>"

USER root

# # user configurations
ENV USER jovyan
# ENV HOME /home/$USER_NAME

# # create user
ARG GROUP_ID=1041
# ARG USER_ID=1001
RUN groupadd -g $GROUP_ID devops
# RUN useradd -d $HOME -m -G sudo -u $USER_ID -g $GROUP_ID -o $USER_NAME

RUN usermod -a -G sudo $USER && usermod -a -G $GROUP_ID $USER

# set user to sudoers (no password required)
RUN echo "$USER ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/99-$USER && chmod 0440 /etc/sudoers.d/99-$USER

RUN apt update && \
  apt install -y -V apt-transport-https curl gnupg lsb-release apt-utils locate \
  ca-certificates \
  git \
  git-lfs \
  libcairo2-dev \
  libczmq-dev \
  libffi-dev \
  libgdbm-dev \
  libgmp-dev \
  liblapacke-dev \
  libmysqlclient-dev \
  libncurses5-dev \
  libopenblas-dev \
  libpoppler-glib-dev \
  libpq-dev \
  libreadline-dev \
  libfftw3-dev \
  libsqlite3-dev \
  libssl-dev \
  libxml2-dev \
  libxslt1-dev \
  libyaml-dev \
  libzmq3-dev \
  libglib2.0-dev \
  gnuplot \
  graphviz \
  mysql-client \
  openssl \
  pkg-config \
  postgresql-client \
  ruby-psych \
  software-properties-common \
  sqlite3 \
  tzdata \
  wget \
  zlib1g-dev && \
  rm -rf /var/lib/apt/lists/*

RUN echo 'deb http://apt.llvm.org/focal/ llvm-toolchain-focal main' >> /etc/apt/sources.list && \
  echo 'deb http://apt.llvm.org/focal/ llvm-toolchain-focal-11 main' >> /etc/apt/sources.list && \
  wget -O - http://llvm.org/apt/llvm-snapshot.gpg.key|apt-key add - && \
  # add-apt-repository -y ppa:ubuntu-toolchain-r/test && \
  apt-get update

ENV APACHE_ARROW_VERSION=10.0.0
RUN apt update && \
  apt install -y --no-install-recommends \
  apt-transport-https \
  lsb-release \
  && \
  \
  curl -sfSL -o /tmp/apache-arrow-apt-source-latest.deb \
  https://apache.jfrog.io/artifactory/arrow/$(lsb_release --id --short | tr 'A-Z' 'a-z')/apache-arrow-apt-source-latest-$(lsb_release --codename --short).deb && \
  apt install -y --no-install-recommends \
  /tmp/apache-arrow-apt-source-latest.deb && \
  rm -f /tmp/apache-arrow-apt-source-latest.deb && \
  \
  apt update && \
  apt install -y \
  libarrow-dev=11.0.0-1 \
  libarrow-cuda-dev=11.0.0-1 \
  libarrow-cuda-glib-dev=11.0.0-1 \
  gir1.2-arrow-cuda-1.0=11.0.0-1 \
  gir1.2-plasma-1.0=11.0.0-1 \
  libarrow-glib-dev=11.0.0-1 \
  libplasma-dev=11.0.0-1 \
  libarrow-dataset-dev=11.0.0-1 \
  libarrow-flight-dev=11.0.0-1 \
  libplasma-glib-dev=11.0.0-1 \
  libgandiva-dev=11.0.0-1 \
  libgandiva-glib-dev=11.0.0-1 \
  libparquet-dev=11.0.0-1 \
  libparquet-glib-dev=11.0.0-1 \
  libgirepository1.0-dev \
  gir1.2-arrow-1.0=11.0.0-1 \
  gir1.2-gandiva-1.0=11.0.0-1 \
  gir1.2-parquet-1.0=11.0.0-1 \
  gir1.2-plasma-1.0=11.0.0-1 \
  && \
  \
  rm -rf /var/lib/apt/lists/*

# Copy Ruby 3.1.3 from rubylang/ruby
COPY --from=rubylang/ruby:3.1.3-focal \
  /usr/local/bin/bundle \
  /usr/local/bin/bundler \
  /usr/local/bin/erb \
  /usr/local/bin/gem \
  /usr/local/bin/irb \
  /usr/local/bin/racc \
  /usr/local/bin/rake \
  /usr/local/bin/rdoc \
  /usr/local/bin/ri \
  /usr/local/bin/ruby \
  /usr/local/bin/

COPY --from=rubylang/ruby:3.1.3-focal \
  /usr/local/etc/gemrc \
  /usr/local/etc/

# NOTE: DO NOT CHANGE the version in the path of include directory
COPY --from=rubylang/ruby:3.1.3-focal \
  /usr/local/include/ruby-3.1.0/ \
  /usr/local/include/ruby-3.1.0/

COPY --from=rubylang/ruby:3.1.3-focal \
  /usr/local/lib/libruby.so \
  /usr/local/lib/libruby.so.* \
  /usr/local/lib/

COPY --from=rubylang/ruby:3.1.3-focal \
  /usr/local/lib/pkgconfig/ \
  /usr/local/lib/pkgconfig/

COPY --from=rubylang/ruby:3.1.3-focal \
  /usr/local/lib/ruby/ \
  /usr/local/lib/ruby/

COPY --from=rubylang/ruby:3.1.3-focal \
  /usr/local/share/man/man1/erb.1 \
  /usr/local/share/man/man1/irb.1 \
  /usr/local/share/man/man1/ri.1 \
  /usr/local/share/man/man1/ruby.1 \
  /usr/local/share/man/man1/

RUN gem update --system 3.4.18

WORKDIR /home/$USER

COPY Gemfile .
COPY gems .

RUN mkdir -pv stuff && chmod 2775 stuff

RUN chown -R $USER:$GROUP_ID /home/$USER

USER $USER

# NOTE: DO NOT CHANGE the version in the path of gem's bin directory
ENV PATH $HOME/.local/share/gem/ruby/3.1.0/bin:$HOME/.local/bin:$PATH

RUN pip install spacy

RUN python3 -m spacy download en_core_web_sm && \
  python3 -m spacy download en_core_web_lg

ENV BUNDLE_PATH $HOME/.local/share/gem
# ENV GEM_HOME $HOME/.local/share/gem/ruby/3.1.0

RUN echo "gem: --user-install" >> $HOME/.gemrc

RUN gem install rbplotly-0.1.2.gem && \
    gem install nmatrix-0.2.4.gem && \
    gem install nmatrix-fftw-0.2.4.gem && \
    gem install nmatrix-lapacke-0.2.4.gem && \
    gem install ferret-0.11.9.0.gem

RUN bundle config build.redic --with-cxx="clang++" --with-cflags="-std=c++0x" && \
  bundle install

# To fix: qt.qpa.xcb could not connect to display
ENV QT_QPA_PLATFORM offscreen

RUN iruby register --force
