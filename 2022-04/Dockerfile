FROM python:3.6 AS py36
RUN apt-get update && apt-get install -y vim
RUN pip install poetry==1.1.13
WORKDIR /repo
COPY .flake8 README.md Makefile delme/pyproject.toml delme/poetry.lock ./
COPY sphinx_multi_theme sphinx_multi_theme
COPY tests tests
COPY docs docs
RUN make init deps
COPY pyproject-min.toml pyproject.toml
COPY poetry-min.lock poetry.lock
RUN make deps
RUN poetry run pip freeze |tee freeze.txt |grep ^Sphinx |tee /dev/stderr |grep -F Sphinx-4.0.
RUN make test

FROM python:3.7 AS py37
RUN apt-get update && apt-get install -y vim
RUN pip install poetry==1.1.13
WORKDIR /repo
COPY .flake8 README.md Makefile delme/pyproject.toml delme/poetry.lock ./
COPY sphinx_multi_theme sphinx_multi_theme
COPY tests tests
COPY docs docs
RUN make init deps
COPY pyproject-min.toml pyproject.toml
COPY poetry-min.lock poetry.lock
RUN make deps
RUN poetry run pip freeze |tee freeze.txt |grep ^Sphinx |tee /dev/stderr |grep -F Sphinx-4.0.
RUN make test

FROM python:3.8 AS py38
RUN apt-get update && apt-get install -y vim
RUN pip install poetry==1.1.13
WORKDIR /repo
COPY .flake8 README.md Makefile delme/pyproject.toml delme/poetry.lock ./
COPY sphinx_multi_theme sphinx_multi_theme
COPY tests tests
COPY docs docs
RUN make init deps
COPY pyproject-min.toml pyproject.toml
COPY poetry-min.lock poetry.lock
RUN make deps
RUN poetry run pip freeze |tee freeze.txt |grep ^Sphinx |tee /dev/stderr |grep -F Sphinx-4.1.
RUN make test

FROM python:3.9 AS py39
RUN apt-get update && apt-get install -y vim
RUN pip install poetry==1.1.13
WORKDIR /repo
COPY .flake8 README.md Makefile delme/pyproject.toml delme/poetry.lock ./
COPY sphinx_multi_theme sphinx_multi_theme
COPY tests tests
COPY docs docs
RUN make init deps
COPY pyproject-min.toml pyproject.toml
COPY poetry-min.lock poetry.lock
RUN make deps
RUN poetry run pip freeze |tee freeze.txt |grep ^Sphinx |tee /dev/stderr |grep -F Sphinx-4.1.
RUN make test

FROM python:3.10 AS py310
RUN apt-get update && apt-get install -y vim
RUN pip install poetry==1.1.13
WORKDIR /repo
COPY .flake8 README.md Makefile delme/pyproject.toml delme/poetry.lock ./
COPY sphinx_multi_theme sphinx_multi_theme
COPY tests tests
COPY docs docs
RUN make init deps
COPY pyproject-min.toml pyproject.toml
COPY poetry-min.lock poetry.lock
RUN make deps
RUN poetry run pip freeze |tee freeze.txt |grep ^Sphinx |tee /dev/stderr |grep -F Sphinx-4.2.
RUN make test

FROM python:3.10 as final
COPY --from=py36 /repo/freeze.txt freeze36.txt
COPY --from=py37 /repo/freeze.txt freeze37.txt
COPY --from=py38 /repo/freeze.txt freeze38.txt
COPY --from=py39 /repo/freeze.txt freeze39.txt
COPY --from=py310 /repo/freeze.txt freeze310.txt
