FROM bioconductor/release_base2:latest
#FROM bioconductor/release_base2:R3.5.1_BioC3.8
# We normally want to build from a versioned image, but the R3.5.1_BioC3.8
# image at this writing (2018-12-07) lacks libtcl8.6, libtk8.6, and appears 
# to have a version of R built without libR.

LABEL project="Standard CTMR R environment"
LABEL version="1.0"
LABEL maintainer="Fredrik Boulund <fredrik.boulund@ki.se>"

RUN install2.r --error \
   beeswarm \
   data.table \
   dplyr \
   dunn.test \
   ggpubr \
   PairedData \
   pheatmap \
   plyr \
   vioplot \
   rafalib \
   RColorBrewer \
   vegan

RUN install2.r remotes --error \
   && \
   installGithub.r benjjneb/dada2

WORKDIR /input
