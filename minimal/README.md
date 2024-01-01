# RubyData base stack

```bash
+--------+
|  ruby  +
+--------+
```

This docker image consists of minimal components for data science using Ruby.

tty-toolkit
langchain, ruby-openai
sequel, pg, mysql2, sqlite3
pry


## Components in this image

- Based on buildpack-deps:xenial
- Ruby 3.1.3
    - Almost same as docker-library's ruby image
- Python 3.10.1
    - Almost same as docker-library's python image
- OpenBLAS
- CZMQ 4.0.2
<!-- - Jupyter Notebook 5.1.x -->
- [tini](https://github.com/krallin/tini) as the container entrypoint
- Python stack
  - numpy, pandas, matplotlib, scipy, seaborn, bokeh, plotly, holoviews,
    scikit-learn, scikit-image, sympy, gensim, nltk, cython, statsmodel,
    patsy, cloudpickle, dill, numba, xray, pyarrow, tensorflow, keras,
    chainer, xgboost

## Basic Usage

This image does not run Jupyter Notebook in default.
The following command runs bash shell on root user.

```
docker run -p 8888:8888 -v ./.env:/home/joyven/.env b08x/minimal:latest
```
