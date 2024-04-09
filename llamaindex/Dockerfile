ARG BASE_IMAGE_TAG=latest
FROM b08x/notebook-minimal:$BASE_IMAGE_TAG

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"

USER $NB_UID

WORKDIR /home/$NB_USER

RUN mamba install --yes 'jupyter-ai' 'google-generativeai' 'huggingface_hub' 'ipython' 'ipywidgets' \
    'llama-index' 'llama-index-cli' 'llama-index-core' \
    'langchain-anthropic' 'langchain-google-genai' 'cohere' \
    'llama-index-embeddings-huggingface'\
    'llama-index-indices-managed-llama-cloud' \
    'llama-index-llms-anthropic' 'llama-index-llms-gemini' 'llama-index-llms-groq' 'llama-index-llms-langchain' \
    'llama-index-llms-localai' 'llama-index-llms-ollama' 'llama-index-llms-openai' 'llama-index-multi-modal-llms-ollama' \
    'llama-index-readers-file' 'llama-index-readers-llama-parse' \
    'llama-index-vector-stores-chroma' \
    'llama-parse' 'nbconvert' 'pillow' 'prompttools' \
    'pydantic' 'pyvis' 'spacy' 'SQLAlchemy' 'torch' 'torchaudio' 'torchvision' 'txtai' && \
    mamba clean --all -f -y && fix-permissions "${CONDA_DIR}" && fix-permissions "/home/${NB_USER}" && \
    python3 -m spacy download en_core_web_trf && \
    python3 -m spacy download en_core_web_lg && \
    python -c "import sys, importlib.util as util; 1 if util.find_spec('nltk') else sys.exit(); import nltk; nltk.download('punkt')"
