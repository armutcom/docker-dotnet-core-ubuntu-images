FROM mcr.microsoft.com/dotnet/core/sdk:3.0.100
ENV PATH="$PATH:/root/.dotnet/tools"
RUN apt-get update && apt-get -y install libdevmapper-dev iptables python-pip && pip install awscli
RUN wget -O containerd.deb -q https://download.docker.com/linux/debian/dists/buster/pool/stable/amd64/containerd.io_1.2.6-3_amd64.deb
RUN dpkg -i containerd.deb
RUN wget -O docker-cl.deb -q https://download.docker.com/linux/debian/dists/buster/pool/stable/amd64/docker-ce-cli_19.03.2~3-0~debian-buster_amd64.deb
RUN dpkg -i docker-cl.deb
RUN wget -O docker.deb -q https://download.docker.com/linux/debian/dists/buster/pool/stable/amd64/docker-ce_19.03.2~3-0~debian-buster_amd64.deb
RUN dpkg -i docker.deb
