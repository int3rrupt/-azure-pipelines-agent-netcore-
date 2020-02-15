 FROM ubuntu:16.04

# To make it easier for build and release pipelines to run apt-get,
# configure apt to not require confirmation (assume the -y argument by default)
ENV DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes

################################################################################################
# Updates and base software
################################################################################################
RUN apt-get update \
&& apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        jq \
        git \
        iputils-ping \
        libcurl3 \
        libicu55 \
        libunwind8 \
        netcat \
        apt-transport-https \
        wget
################################################################################################
# Additional Repositories
################################################################################################
# Download the Microsoft repository GPG keys
RUN wget -q https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
# Register the Microsoft repository GPG keys
RUN dpkg -i packages-microsoft-prod.deb
# Update the list of products
RUN apt-get update
################################################################################################
# Additional software
################################################################################################
RUN apt-get install -y --no-install-recommends \
        unzip \
        powershell

WORKDIR /opt/cssp
RUN mkdir sqlpackage
# Add sqlpackage to PATH
RUN echo "export PATH=\"\$PATH:/opt/cssp/sqlpackage\"" >> $HOME/.bashrc
# Downlaod sqlpackage
RUN wget --content-disposition https://go.microsoft.com/fwlink/?linkid=2113331
# Get filename
RUN fileName=$(ls | grep sqlpackage*.zip) \
        # Get sqlpackage version and set sqlpackage environment variable
        && echo "export sqlpackage=$(echo $fileName | grep -Eio '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]*')" >> $HOME/.bashrc \
        # Extract sqlpackage
        && unzip $fileName -d ./sqlpackage \
        # Make sqlpackage executable
        && chmod a+x ./sqlpackage/sqlpackage \
        # && source .bashrc \
        # Remove downloaded zip
        && rm $fileName

WORKDIR /azp

COPY ./start.sh .
RUN chmod +x start.sh

CMD ["./start.sh"]