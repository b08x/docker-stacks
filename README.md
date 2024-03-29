# Docker Stacks: Jupyter Python Ruby

[![CI](https://github.com/b08x/docker-stacks/actions/workflows/ci.yml/badge.svg?branch=development)](https://github.com/b08x/docker-stacks/actions/workflows/ci.yml)

The RubyData Jupyter Notebook Docker Stack is designed to provide a seamless environment for data processing and analysis using Ruby and Jupyter notebooks. This stack includes a curated selection of essential Ruby gems to facilitate efficient data extraction, transformation, and loading (ETL) processes.

RubyData Docker stacks are a set of ready-to-run Docker images containing Jupyter and data tools for Rubyists.

This repository is based on [rubydata/docker-stacks](https://github.com/RubyData/docker-stacks).


## Quick Start

### Minimal Image

### LLM Image

### LlamaStuff Image

### NLP Image


minimal --> llamaindex --> nlp 

minimal -> audio




Contains gems necessary to build an application working with Large Language Models (LLMs).

Question Answering System: Building a system that can answer questions based on a knowledge base stored in Chroma.

Text Summarization or Generation Tool: Leveraging LLMs to summarize larger pieces of text or generate new content based on prompts.

Intelligent Search: Creating a search interface allowing the LLMs to understand natural language queries and find suitable matches within the knowledge base.

# Building Images

## Using the Rakefile

The Rakefile defines the following key tasks:

- pull/base_image/{image} - Pulls the base image for each supported image (minimal, datascience, etc.)
- build/{image} - Builds the Docker image, depends on pulling the base image
- tag/{image} - Tags the built image with the Git commit sha
- push/{image} - Pushes the image to the Docker registry

It also defines composite tasks:

- build-all - Builds all images in parallel
- tag-all - Tags all images
- push-all - Pushes all images

These tasks leverage variables defined at the top like ALL_IMAGES, BASE_IMAGES, OWNER to programmatically handle building, tagging and pushing multiple images.

Individual tasks are invoked for each image, while the composite tasks iterate over ALL_IMAGES to perform the action for all of them.

This Rakefile automation handles common repetitive Docker image operations and allows building, tagging and pushing multiple images with simple rake commands.

## License

MIT License
