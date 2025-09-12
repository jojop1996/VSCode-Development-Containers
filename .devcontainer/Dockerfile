FROM 

## This is a devcontainer I use to perform general tasks
## It includes a variety of tools and settings for testing different applications and automations

# Node CA
## VSCode server uses node
## This allows VSCode to use the Select Certificates for downloading extensions
ENV NODE_EXTRA_CA_CERTS=/etc/ssl/certs/ca-certificates.crt

# Set the Ansible Python interpreter to use python3 (3.9.xx) as 3.10+ is not supported
ENV ANSIBLE_PYTHON_INTERPRETER="/usr/bin/python3"

# Set so the VSCode server knows what python interpreter to use
ENV PYTHONPATH="/bin/python"

# Python CA
## This was added to the Dynatrace sdk can reach out to the SaaS
ENV REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt

# Updates
RUN apt-get update && apt-get install -y

# Install editor, compile, and network tools
RUN apt install -y nano iputils-ping dnsutils xz-utils python3-pip wget dos2unix net-tools telnet maven

# Set noninteractive mode for apt-get
## Added because the next install for krb5-user was still prompting for user input
ENV DEBIAN_FRONTEND=noninteractive

# Install the Kerberos User library for Windows authentication
RUN apt-get install krb5-user libkrb5-dev sshpass -y

# Install pywinrm and pykerberos for Windows remote executions
RUN pip install ansible pywinrm ansible-lint pywinrm[kerberos] hvac

# Set locales
RUN apt install locales
RUN export LC_ALL=en_US.UTF-8
RUN export LANG=en_US.UTF-8
RUN locale-gen en_US.UTF-8

# Set environment variables
ENV JAVA_HOME=/apps/products/jdk/jdk-17.0.14
ENV PATH=$JAVA_HOME/bin:$PATH
ENV _JAVA_OPTIONS="-Djavax.net.ssl.trustStore=/etc/ssl/certs/java/cacerts"

# Create the target directory
RUN mkdir -p /apps/products/jdk && chmod 0755 /apps/products/jdk

# Copy the JDK tarball into the image
COPY ./java/jdk-17.0.14_linux-x64_bin.tar.gz /tmp/jdk.tar.gz

# Extract the tarball to the target directory
RUN tar -xzf /tmp/jdk.tar.gz -C /apps/products/jdk/ && rm /tmp/jdk.tar.gz