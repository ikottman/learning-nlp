FROM jupyter/datascience-notebook

# install spacy and english model
RUN conda install --channel conda-forge spacy && \
    python -m spacy download en

RUN conda install nltk