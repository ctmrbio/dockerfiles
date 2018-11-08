FROM bioconductor/release_base2

LABEL project="yue_docker"
LABEL version="develop"

MAINTAINER Yue Hu "yue.hu@ki.se"

RUN R -e 'install.packages("devtools"); devtools::install_github("benjjneb/dada2")'
