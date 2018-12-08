FROM bioconductor/release_base2:R3.5.1_Bioc3.8

LABEL project="Standard CTMR R environment"
LABEL version="1.1"
LABEL maintainer="Fredrik Boulund <fredrik.boulund@ki.se>"

# Due to agressive slimming of bioconductor/release_base2:R3.5.1_Bioc3.8, the
# Tcl/Tk dev packages have been removed. We therefore install the tcl/tk dev
# packages, as those are required to build "sm", which "vioplot" depends on.
# See https://github.com/rocker-org/rocker/issues/316
RUN apt-get update \
   && \
   apt-get -y install \
      tcl8.6-dev \
	  tk8.6-dev

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

# We need to install "remotes" in order to install DADA2 from github sources.
RUN install2.r remotes --error \
   && \
   installGithub.r benjjneb/dada2

WORKDIR /input
