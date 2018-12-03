# dockerfiles
Dockerfiles used at CTMR that are not coupled to a specific project.

## rstudio_luisa

- Based on `bioconductor/release_base2`
- DADA2 installed from github sources
- Working directory is `/home/rstudio`

Example command:

```
docker run -d --rm -it -u $(id -u):$(id -g) -p 8787:8787 -v $pwd:/home/rstudio ctmrbio/rstudio_dada2
```

Use this image with SSH port forwarding to access the RStudio interface running
inside the container. The default hosting port inside the container is `8787`. 

## rstudio_dada2

- Based on `bioconductor/release_base2`
- DADA2 installed from github sources
- Working directory is `/home/rstudio`

Example command:

```
docker run --rm -it -u $(id -u):$(id -g) -p 8787:8787 -v $pwd:/home/rstudio ctmrbio/rstudio_dada2
```

Use this image with SSH port forwarding to access the RStudio interface running
inside the container. The default hosting port inside the container is `8787`. 


## picrust2

- Based on `ubuntu:18.04`
- Builds and installs `epa-ng`, and `gappa`
- Working directory is `/mnt`

```
docker run --rm -it -u $(id -u):$(id -g) -v $pwd:/picrust2 ctmrbio/picrust2
```

NOTE: You have to run `source activate picrust2` to activate the PICRUSt2 conda
environment after launching the docker container.
