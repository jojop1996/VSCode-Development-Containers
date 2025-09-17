FROM ubuntu:jammy

# General environment variables
ENV NODE_EXTRA_CA_CERTS=/etc/ssl/certs/ca-certificates.crt \
    ANSIBLE_PYTHON_INTERPRETER="/usr/bin/python3" \
    PYTHONPATH="/bin/python" \
    REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt \
    DEBIAN_FRONTEND=noninteractive \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8

# Update and install wget
RUN apt-get update && \
    apt-get install -y \
        wget gpg

RUN wget -O - https://apt.releases.hashicorp.com/gpg | gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list

# Update and install packages (including terraform and packer after adding HashiCorp repo)
RUN apt-get update && \
    apt-get install -y \
        nano iputils-ping dnsutils xz-utils python3-pip dos2unix net-tools telnet maven \
        krb5-user libkrb5-dev sshpass locales terraform packer && \
    locale-gen en_US.UTF-8

# Install Python packages
RUN pip install ansible pywinrm ansible-lint pywinrm[kerberos] hvac