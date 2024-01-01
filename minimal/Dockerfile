ARG BASE_IMAGE_TAG=f3079808ca8c
FROM jupyter/scipy-notebook:$BASE_IMAGE_TAG

LABEL maintainer="Kenta Murata <mrkn@mrkn.jp>"

USER root

ENV USER jovyan

ARG GROUP_ID=1041

RUN groupadd -g $GROUP_ID devops

RUN usermod -a -G sudo $USER && usermod -a -G $GROUP_ID $USER

RUN echo "$USER ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/99-$USER && chmod 0440 /etc/sudoers.d/99-$USER

# Pre-requisites
RUN apt-get update && \
  apt-get install -y -V --no-install-recommends \
  apt-transport-https \
  gnupg lsb-release apt-utils locate \
  ca-certificates \
  curl \
  cmake \
  gcc \
  git \
  git-lfs \
  libczmq-dev \
  libffi-dev \
  libgdbm-dev \
  libgmp-dev \
  liblapacke-dev \
  libmysqlclient-dev \
  libncurses5-dev \
  libopenblas-dev \
  libpq-dev \
  libreadline-dev \
  libsqlite3-dev \
  libssl-dev \
  libxml2-dev \
  libxslt1-dev \
  libyaml-dev \
  libzmq3-dev \
  neovim \
  openssl \
  ruby-psych \
  software-properties-common \
  mysql-client \
  pkg-config \
  postgresql-client \
  sqlite3 \
  tzdata \
  zlib1g-dev \
  wget && \
  apt-get clean && rm -rf /var/lib/apt/lists/*

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

WORKDIR /home/$USER

COPY minimal/Gemfile .

RUN mkdir -pv output && chmod 2775 output

RUN echo "[[ -f ~/.env ]] && source ~/.env" >> /home/$USER/.bashrc

RUN chown -R $USER:$GROUP_ID /home/$USER

USER $NB_UID

RUN echo "gem: --user-install" >> $HOME/.gemrc

# NOTE: DO NOT CHANGE the version in the path of gem's bin directory
ENV PATH $HOME/.local/share/gem/ruby/3.1.0/bin:$PATH

ENV BUNDLE_PATH $HOME/.local/share/gem

RUN bundle install

RUN iruby register --force
