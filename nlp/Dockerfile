ARG BASE_IMAGE_TAG=f3079808ca8c
FROM jupyter/datascience-notebook:$BASE_IMAGE_TAG
LABEL maintainer="Kenta Murata <mrkn@mrkn.jp>"

USER root

ENV USER jovyan

ARG GROUP_ID=1041

RUN groupadd -g $GROUP_ID devops

RUN usermod -a -G sudo $USER && usermod -a -G $GROUP_ID $USER

RUN echo "$USER ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/99-$USER && chmod 0440 /etc/sudoers.d/99-$USER

ENV APACHE_ARROW_VERSION=10.0.0

RUN apt update && \
  apt install -y -V --no-install-recommends apt-transport-https \
  curl gnupg lsb-release apt-utils locate \
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
  echo 'deb http://apt.llvm.org/focal/ llvm-toolchain-focal main' >> /etc/apt/sources.list && \
  echo 'deb http://apt.llvm.org/focal/ llvm-toolchain-focal-11 main' >> /etc/apt/sources.list && \
  wget -O - http://llvm.org/apt/llvm-snapshot.gpg.key|apt-key add - && \
  apt-get update -qq && \
  apt-get autoremove -y && \
  rm -rf /var/lib/apt/lists/*

RUN mkdir -pv /home/$USER/packages

COPY packages packages/

RUN curl -sfSL -o /tmp/apache-arrow-apt-source-latest.deb \
  https://apache.jfrog.io/artifactory/arrow/$(lsb_release --id --short | tr 'A-Z' 'a-z')/apache-arrow-apt-source-latest-$(lsb_release --codename --short).deb && \
  apt install -y --no-install-recommends \
  /tmp/apache-arrow-apt-source-latest.deb && \
  rm -f /tmp/apache-arrow-apt-source-latest.deb && \
  apt update -qq && \
  apt install -y \
  libgirepository1.0-dev

RUN ls -lah && apt install -y --no-install-recommends \
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

RUN gem update --system 3.4.22

WORKDIR /home/$USER

COPY Gemfile .
#TODO: Create gemspec for ferret
COPY gems/ferret-0.11.9.0.gem .

RUN mkdir -pv output && chmod 2775 output

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
