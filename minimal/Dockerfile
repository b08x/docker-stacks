ARG BASE_IMAGE_TAG=latest
FROM quay.io/jupyter/datascience-notebook:$BASE_IMAGE_TAG

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"
LABEL contributor="Kenta Murata <mrkn@mrkn.jp>"

USER root

ARG GROUP=1041

RUN groupadd -g $GROUP notebook && \
    usermod -a -G sudo $NB_USER && usermod -a -G $GROUP $NB_USER && \
    echo "$NB_USER ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/99-$NB_USER && \
    chmod 0440 /etc/sudoers.d/99-$NB_USER

# Pre-requisites
RUN apt-get update && apt-get install -y -V --no-install-recommends dialog gnupg gpg-agent \
    apt-transport-https gnupg lsb-release apt-utils apt-transport-https ca-certificates && \
    echo 'deb http://apt.llvm.org/focal/ llvm-toolchain-focal main' >> /etc/apt/sources.list && \
    echo 'deb http://apt.llvm.org/focal/ llvm-toolchain-focal-11 main' >> /etc/apt/sources.list && \
    wget -O llvm-snapshot.gpg.key http://llvm.org/apt/llvm-snapshot.gpg.key && \
    apt-key add llvm-snapshot.gpg.key && \
    apt-get update -qq && apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get update -qq && apt-get install -y -V --no-install-recommends \
      cmake \
      curl \
      exiftool \
      gcc \
      ghostscript \
      git \
      git-lfs \
      gnuplot \
      graphviz \ 
      jruby \
      libcairo2-dev \
      libczmq-dev \
      libffi-dev \
      libfftw3-dev \
      libgdbm-dev \
      libgirepository1.0-dev \
      libglib2.0-dev \
      libgmp-dev \
      libgsl-dev \
      liblapacke-dev \
      libmagick++-dev \
      libmysqlclient-dev \
      libncurses5-dev \
      libopenblas-dev \
      libplot2c2 \
      libpoppler-glib-dev \
      libpq-dev \
      libreadline-dev \
      libreoffice \
      libsqlite3-dev \
      libssl-dev \
      libtamuanova-0.2 \
      libxml2-dev \
      libxslt1-dev \
      libyaml-dev \
      libzmq3-dev \
      liblink-grammar-dev \
      link-grammar \
      minisat \
      mysql-client \
      neovim \
      openjdk-21-jdk-headless \
      openssl \
      pandoc \
      pdftk \
      pkg-config \
      plotutils \
      poppler-utils \
      postgresql-client \
      python3-link-grammar \
      ruby-psych \
      rsync \
      software-properties-common \
      sqlite3 \
      tesseract-ocr \
      tidy \
      tzdata \
      zip \     
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

RUN echo "gem: --user-install" >> /etc/gemrc && \
    echo "gem: --no-user-install --no-document" >> /root/.gemrc && \
    gem update --system --platform x86_64-linux && gem clean

USER $NB_UID

RUN echo "[[ -f ~/.env ]] && source ~/.env" >> $HOME/.bashrc && \
    echo "gem: --user-install" >> $HOME/.gemrc

# NOTE: DO NOT CHANGE the version in the path of gem's bin directory
ENV PATH $HOME/.local/share/gem/ruby/3.1.0/bin:$PATH

# Install basic gems
RUN \
    gem install irb && \
    gem install iruby -v ">= 0.7.4" && \
    gem install pycall -v ">= 1.5.1" && \
    gem install \
        activesupport \
        awesome_print \
        bcrypt_pbkdf \
        bundler \
        childprocess \
        colorize \
        coltrane \
        debug \
        dry-initializer \
        dry-monads \
        ed25519 \
        eventmachine \
        ffi \
        fractional \
        geo_coord \
        hashie \
        highline \
        i18n \
        json \
        jsonl \
        kramdown \
        logging \
        minitest \
        mocha \
        multi_json \
        mysql2 \
        net-ssh \
        open3 \
        open4 \
        parallel \
        pastel \
        pg \
        pry \
        pry-doc \
        psych \
        rake \
        rdoc \
        redcarpet \
        rexml \
        rouge \
        rugged \
        ruport \
        sequel \
        sqlite3 \
        sync \
        sys-proctable \
        treetop \
        tty-box \
        tty-command \
        tty-config \
        tty-markdown \
        tty-prompt \
        tty-screen \
        yaml

RUN iruby register --force
