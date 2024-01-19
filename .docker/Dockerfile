FROM gitpod/openvscode-server:1.85.1

LABEL maintainer="@k33g"

# Set environment variables for NodeJS
ARG NODE_DISTRO=${NODE_DISTRO}
ARG NODE_VERSION=${NODE_VERSION}

# To get permissions to install packages and such
USER root 

# ------------------------------------
# Install Tools
# ------------------------------------
RUN <<EOF
apt-get update 
apt-get install -y build-essential xz-utils software-properties-common 
apt-get install -y htop openssh-server
apt-get install gopls delve pkg-config libssl-dev sshpass gettext
apt-get install -y bat exa hey
ln -s /usr/bin/batcat /usr/bin/bat
EOF

# ------------------------------------
# Install NodeJS
# ------------------------------------
RUN <<EOF
wget https://nodejs.org/dist/${NODE_VERSION}/node-${NODE_VERSION}-${NODE_DISTRO}.tar.xz
mkdir -p /usr/local/lib/nodejs
tar -xJvf node-$NODE_VERSION-$NODE_DISTRO.tar.xz -C /usr/local/lib/nodejs
rm node-$NODE_VERSION-$NODE_DISTRO.tar.xz
EOF

ENV VERSION="${NODE_VERSION}"
ENV DISTRO="${NODE_DISTRO}"
ENV PATH=/usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin:$PATH

# ------------------------------------
# Install Docker
# ------------------------------------
RUN <<EOF
# Add Docker's official GPG key:
apt-get update
apt-get install -y ca-certificates curl gnupg
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null
apt-get update
apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
EOF

RUN <<EOF
groupadd docker
usermod -aG docker openvscode-server
EOF

# to restore permissions for the web interface
USER openvscode-server 

