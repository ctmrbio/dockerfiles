# dockerfiles
Dockerfiles used at CTMR that are not coupled to a specific project.

Typical Docker run invocations look like this:

```
docker run <options> ctmrbio/container_name
```

The following options are often used:

- `--rm`: remove container after it's closed (you pretty much always want this).
- `-i`: Interactive use, connect your terminal to a terminal in the container.
- `-t`: Create a pseudo-TTY so you get nice things such as tab completion etc.
- `-u <username>:<group>`: Set username and group to run as inside the container 
  (e.g. `-u boulund:ctmrbioinfo`).  Commands often show `-u $(id -u):$(id -g)`, 
  which will call `id` to automatically get your user and group ID. Note that
  the group ID might be incorrect if working in a shared folder.
- `-p`: Connect a port on the host (e.g. CTMR-NAS) to a port in the container.
  Often used for connecting to web services inside a container, such as RStudio
  or Pavian.
- `-v <host_path>:<container_path>`: Mount a path on the host machine into the
  container. The `<container_path>` doesn't have to exists beforehand. Can be 
  useful to connect e.g. an output directory as `-v /path/to/outdir:/outdir`.


## rstudio_luisa

- Based on `bioconductor/release_base2`
- Working directory is `/home/rstudio`
- Installed packages:
  - vegan
  - RColorBrewer
  - vioplot
  - pheatmap

Example command:

```
docker run -d --rm -it -u $(id -u):$(id -g) -p 8787:8787 -v $pwd:/home/rstudio ctmrbio/rstudio_luisa
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

NOTE: The image uses an ENTRYPOINT that is script located in `/picrust2/activate_picrust2_env.sh`
that automatically activates the `picrust2` conda environment inside the container.


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
