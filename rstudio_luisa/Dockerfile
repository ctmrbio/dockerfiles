FROM bioconductor/release_base2:latest

LABEL project="Luisa CTMR RStudio image"
LABEL version="0.1-dev"
LABEL maintainer="Luisa Hugerth <luiwar@ki.se>"

RUN R -e 'install.packages("vegan");install.packages("pheatmap");install.packages("vioplot");install.packages("RColorBrewer")'
