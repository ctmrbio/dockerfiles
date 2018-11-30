# dockerfiles
Dockerfiles used at CTMR that are not coupled to a specific project.

## `rstudio_dada2`

- Based on `bioconductor/release_base2`
- DADA2 installed from github sources
- Working directory is `/home/rstudio`

Example command:

```
docker run --rm -it -u $(id -u):$(id -g) -v $pwd:/home/rstudio ctmrbio/rstudio_dada2
```

To run the DADA2 workflow, run `dada2.R RUN_NAME`. It will look for read files in
the current working directory (i.e. the `$pwd` component in the `-v` argument to docker.

## picrust2

- Based on `ubuntu:18.04`
- Builds and installs `epa-ng`, and `gappa`
- Working directory is `/mnt`

```
docker run --rm -it -u $(id -u):$(id -g) -v $pwd:/picrust2 ctmrbio/picrust2
```

NOTE: You have to run `source activate picrust2` to activate the PICRUSt2 conda
environment after launching the docker container.
