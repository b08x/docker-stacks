# RubyData Jupyter Docker Stacks

[![CI](https://github.com/b08x/docker-stacks/actions/workflows/ci.yml/badge.svg?branch=development)](https://github.com/b08x/docker-stacks/actions/workflows/ci.yml)

The RubyData Jupyter Notebook Docker Stack is designed to provide a seamless environment for data processing and analysis using Ruby and Jupyter notebooks. This stack includes a curated selection of essential Ruby gems to facilitate efficient data extraction, transformation, and loading (ETL) processes.

RubyData Docker stacks are a set of ready-to-run Docker images containing Jupyter and data tools for Rubyists.

This repository is based on [rubydata/docker-stacks](https://github.com/RubyData/docker-stacks).


## Quick Start

### Minimal Image

### Data Science Image

### NLP Image

## How to run on docker in your machine

The following command pulls the latest `b08x/datascience-notebook` image from DockerHub if it isn't available on the local machine.  It then starts a container running a Jupyter Notebook system and exposes the 8888 port so that you can connect to Jupyter Notebook.  You can see the URL of `http://<hostname>:8888/?token=<token>` in the server log in the terminal.  Visiting this URL in a Web browser, you can see Jupyter Notebook dashboard page on the browser.

```
docker run -p 8888:8888 b08x/datascience-notebook
```

## Lisence

MIT License
