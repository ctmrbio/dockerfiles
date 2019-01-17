FROM bioconductor/release_base2:R3.5.2_Bioc3.8

LABEL project="Standard CTMR R environment"
LABEL version="2.3"
LABEL maintainer="Fredrik Boulund <fredrik.boulund@ki.se>"

RUN apt-get update \
   && \
   apt-get -y install \
	  python3-pandas \
	  python3-pip \
	  python3-scipy \
	  python3-seaborn \
   && \
   pip3 install \
      jupyter \
	  xlrd \
	  openpyxl 

RUN install2.r --error \
   ape \
   beeswarm \
   data.table \
   dunn.test \
   ggpubr \
   pheatmap \
   rafalib \
   tidyverse \
   vegan \
   vioplot \
   PairedData \
   RColorBrewer

# We need to install "remotes" in order to install DADA2 from github sources.
RUN install2.r remotes --error \
   && \
   installGithub.r benjjneb/dada2

RUN installGithub.r \
   IRkernel/repr \
   IRkernel/IRdisplay \
   IRkernel/IRkernel \
   && \
   R -e 'IRkernel::installspec(user = FALSE)'

COPY start_jupyter /usr/bin

# Workaround to enable users to run Jupyter notebook with non-existing users.
# Jupyter tries to resolve a user's home dir, so when running Docker with `-u` and
# setting a user ID that does not exist inside the container will lead to
# Jupyter not being able to resolve `~/.local` properly, it will resolve `~` as an
# empty string, and subsequently tries to write to `/.local`
RUN mkdir /.local; chmod a+w /.local

WORKDIR /input
