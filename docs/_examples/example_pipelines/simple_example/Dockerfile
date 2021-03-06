# Generated by Porcupine
# Modelled after Neurodocker v0.2.0-dev. https://github.com/kaczmarj/neurodocker

FROM timvanmourik/porcupine

#----------------------------------------------------------
# Install common dependencies and create default entrypoint
#----------------------------------------------------------
ARG DEBIAN_FRONTEND=noninteractive
ENV LANG="C.UTF-8" \
    LC_ALL="C" \
    ND_ENTRYPOINT="/porcupine/startup.sh"
RUN apt-get update -qq && apt-get install -yq --no-install-recommends  \
    	bzip2 ca-certificates curl unzip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && chmod 777 /opt && chmod a+s /opt \
    && mkdir /porcupine \
    && echo '#!/usr/bin/env bash' >> $ND_ENTRYPOINT \
    && echo 'set +x' >> $ND_ENTRYPOINT \
    && echo 'if [ -z "$*" ]; then /usr/bin/env bash; else $*; fi' >> $ND_ENTRYPOINT \
    && chmod -R 777 /porcupine && chmod a+s /porcupine
ENTRYPOINT ["/porcupine/startup.sh"]

#-----------------------------------------------------------
# Install FSL v5.0.10
# FSL is non-free. If you are considering commerical use
# of this Docker image, please consult the relevant license:
# https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/Licence
#-----------------------------------------------------------
RUN echo "Downloading FSL ..." \
    && curl -sSL https://fsl.fmrib.ox.ac.uk/fsldownloads/fsl-5.0.10-centos6_64.tar.gz \
    | tar zx -C /opt \
    && /bin/bash /opt/fsl/etc/fslconf/fslpython_install.sh -q -f /opt/fsl \
    && sed -i '$iecho Some packages in this Docker container are non-free' $ND_ENTRYPOINT \
    && sed -i '$iecho If you are considering commercial use of this container, please consult the relevant license:' $ND_ENTRYPOINT \
    && sed -i '$iecho https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/Licence' $ND_ENTRYPOINT \
    && sed -i '$isource $FSLDIR/etc/fslconf/fsl.sh' $ND_ENTRYPOINT
ENV FSLDIR=/opt/fsl \
    PATH=/opt/fsl/bin:$PATH

#--------------------------------
# Add custom analysis file as CMD
#--------------------------------
ADD /path/to/pipeline/script.py /somewhere/porcupipeline.py
CMD['python', '/somewhere/porcupipeline.py']
