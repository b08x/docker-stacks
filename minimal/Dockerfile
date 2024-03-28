ARG BASE_IMAGE_TAG=f3079808ca8c
FROM jupyter/scipy-notebook:$BASE_IMAGE_TAG

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"
LABEL contributor="Kenta Murata <mrkn@mrkn.jp>"

USER root

ARG GROUP=1041

RUN groupadd -g $GROUP notebook && \
    usermod -a -G sudo $NB_USER && usermod -a -G $GROUP $NB_USER && \
    echo "$NB_USER ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/99-$NB_USER && \
    chmod 0440 /etc/sudoers.d/99-$NB_USER

# Pre-requisites
RUN apt-get update && apt-get install -y -V --no-install-recommends gnupg gpg-agent \
    apt-transport-https gnupg lsb-release apt-utils apt-transport-https ca-certificates && \
    echo 'deb http://apt.llvm.org/focal/ llvm-toolchain-focal main' >> /etc/apt/sources.list && \
    echo 'deb http://apt.llvm.org/focal/ llvm-toolchain-focal-11 main' >> /etc/apt/sources.list && \
    wget -O llvm-snapshot.gpg.key http://llvm.org/apt/llvm-snapshot.gpg.key && \
    apt-key add llvm-snapshot.gpg.key && \
    apt-get update -qq && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get update && apt-get install -y -V --no-install-recommends \
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
      pandoc \
      ruby-psych \
      software-properties-common \
      mysql-client \
      pkg-config \
      postgresql-client \
      sqlite3 \
      tzdata \
      zlib1g-dev \
      wget && \
   apt-get clean

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

WORKDIR /home/$NB_USER

COPY minimal/Gemfile .

RUN echo "[[ -f ~/.env ]] && source ~/.env" >> /home/$NB_USER/.bashrc && \
    echo "gem: --user-install" >> /home/$NB_USER/.gemrc && \
    echo "gem: --user-install" >> /etc/gemrc && \
    echo "gem: --no-user-install --no-document" >> /root/.gemrc && \
    gem update --system 3.5.7

RUN chown -R $NB_UID:$GROUP /home/$NB_USER

USER $NB_UID

# NOTE: DO NOT CHANGE the version in the path of gem's bin directory
ENV PATH $HOME/.local/share/gem/ruby/3.1.0/bin:$PATH
ENV BUNDLE_PATH $HOME/.local/share/gem

RUN bundle install

RUN iruby register --force
