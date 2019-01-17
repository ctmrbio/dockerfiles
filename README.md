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
- `-u <userid>:<groupid>`: Set user id  and group id to run as inside the
  container. Commands often show `-u $(id -u):$(id -g)`, which will call `id`
  to automatically get your user and group id. Note that the group id might be
  incorrect if working in a shared folder. For work in our shared CTMR folders,
  set the group ID to `1314` (the group id for `ctmrbioinfo`). **NOTE:** This
  is not how to set user and group ID when running RStudio in the
  ctmrbio/rstudio container! See details below.
- `-p <host_port>:<container_port>`: Connect a port on the host (e.g. CTMR-NAS) 
  to a port in the container. Often used for connecting to web services inside 
  a container, such as RStudio or Pavian. The `<host_port`> can be exchanged for
  pretty much whatever you want above `1024`, but the `<container_port>` usually 
  has to be set to a specific value, depending on what service is running inside 
  the container.
- `-v <host_path>:<container_path>`: Mount a path on the host machine into the
  container. The `<container_path>` doesn't have to exists beforehand. Can be 
  useful to connect e.g. an output directory as `-v /path/to/outdir:/outdir`.
- `-d`: Detach. This is used for images that run/host a service that is
  acessible without a terminal, e.g. when running an RStudio container that you
  access via your browser using port forwarding into the container. Remember to
  `docker kill <container_name>` when you are done working with your container!
  Otherwise it will keep running in the background (forever).

## rstudio -- The standard CTMR R environment

- Based on `bioconductor/release_base2`
- Working directory is `/input`
- Installed packages:
  - tidyverse (ggplot2, dplyr, tidyr, readr, purrr, tibble, strigr, etc.)
  - ape
  - beeswarm
  - dada2
  - data.table
  - dunn.test
  - ggpubr
  - PairedData
  - pheatmap
  - vioplot
  - rafalib
  - RColorBrewer
  - vega
- Jupyter
- Basic python3 data analysis stack:
  - scipy
  - numpy
  - pandas
  - xlrd
  - openpyxl

The RStudio image is a bit special, as it is built on official Bioconductor
image that has some unique implementations to simplify the use of RStudio
inside the container. To specify user and group ID when running RStudio inside
the container you use `-e USERID=<userid>` and `-e GROUPID=<groupid>` when
launching the container, instead of the normal `-u <userid>:<groupid>`
argument.

Also note that this image **requires** that you set a password used to login to
RStudio inside the container. This is done by setting an environment variable,
`PASSWORD`, when launching the container: `-e PASSWORD=<your_password>`. The
password **cannot** be `rstudio`. The image will shut down directly if no
password is set. The username used to login to RStudio in the container is
`rstudio`.

Example command:

```
docker run -d --rm -it -e PASSWORD=ctmrbio -e USERID=$(id -u) -e GROUPID=1314 -p 8787:8787 -v $(pwd):/input ctmrbio/rstudio
```

Replace `ctmrbio` in the above command with a password of your choice. The
`USERID` is set using the command `id -u`, which is replaced by the ID of your
current user. The `GROUPID` variable can also be replaced with a GID of your
choice if you want to run it in a folder that is not one of the shared CTMR
folders, or on a different system.

Use this image with SSH port forwarding to access the RStudio interface running
inside the container. The default hosting port inside the container is `8787`. 

The tag `ctmrbio/rstudio:latest` will always refer to the most recent
R/Bioconductor version. Version numbering of this container otherwise works
like this:

    ctmrbio/rstudio:<image_version>_R<R_version>_Bioc<Bioconductor_version>


### Jupyter
The `rstudio` image also comes with Jupyter installed with kernels for Python 3
and  R. There is a script installed in the image called `start_jupyter` that
starts a notebook server in the current directory (the default is `/input`
inside the container). Note that the Jupyter session will not be able to access
things outside of the directory you mount into `/input` when launching the
container. Also remember to activate a port forward from a port of your choice
to the default Jupyter port of `8888` inside the container.

```
docker run --rm -it -u $(id -u):1314 -p 8888:8888 -v $(pwd):/input ctmrbio/rstudio start_jupyter
```

Also note that when using Jupyter, you **must** use `-u <userid>:<groupid>`
when launching the container, otherwise all files and folders created by the
Jupyter session will be owned by root.


## picrust2

- Based on `ubuntu:18.04`
- Working directory is `/mnt`
- Builds and installs:
  - `epa-ng`
  - `gappa`
  - `picrust2` conda environment

Example command:

```
docker run --rm -it -u $(id -u):$(id -g) -v $pwd:/mnt ctmrbio/picrust2
```

NOTE: The image uses an ENTRYPOINT script that is located in
`/picrust2/activate_picrust2_env.sh` that automatically activates the
`picrust2` conda environment inside the container.


# Deprecated images
Notes for older Docker images that are currently considered deprecated.

## rstudio_luisa [DEPRECATED]

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

DEPRECATED - replaced by ctmrbio/rstudio.

## rstudio_dada2 [DEPRECATED]

- Based on `bioconductor/release_base2`
- DADA2 installed from github sources
- Working directory is `/home/rstudio`

Example command:

```
docker run --rm -it -u $(id -u):$(id -g) -p 8787:8787 -v $pwd:/home/rstudio ctmrbio/rstudio_dada2
```

Use this image with SSH port forwarding to access the RStudio interface running
inside the container. The default hosting port inside the container is `8787`. 

DEPRECATED - replaced by ctmrbio/rstudio.
