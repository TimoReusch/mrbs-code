# Documentation
The documentation is created using [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

## Running a development server
The folder includes a `Dockerfile`. You can create a container like this:

```bash
docker build -t mrbs-mkdocs . 
```

Start the container by executing

```bash
docker run --rm -it -p 8000:8000 -v ${PWD}:/docs mrbs-mkdocs
```

This will start a development-server on [localhost:8000](https://localhost:8000). The pages automatically refresh on saving.