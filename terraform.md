

#TFENV

## tfenv 
### installation

`git clone --depth=1 https://github.com/tfutils/tfenv.git ~/.tfenv`
echo 'export PATH=$PATH:$HOME/.tfenv/bin' >> ~/.bashrc
tfenv list
tfenv install X.Y.Z
tfenv use X.Y.Z
tfenv version-name
`
NOTE: if the project contains  .terraform-version, tfenv will change automatically to the Terraform version defined in this file.

# Launch Terraform inside a Docker Container

# Docker

## Dockerfile

`FROM debian:10

ENV DEBIAN_FRONTEND noninteractive
RUN apt update -qq \
    && apt install -qq -y --no-install-recommends \
       wget curl unzip git less net-tools \
       ca-certificates jq\
       apt-transport-https lsb-release gnupg \
       && rm -rf /var/lib/apt/lists/*

# Install postgresql-client-12
RUN echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list
RUN curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -

RUN apt update -qq \
 && apt install -qq -y --no-install-recommends postgresql-client-12 \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /tmp

# Install Terraform
ARG TERRAFORM_VERSION=0.13.6
ENV TERRAFORM_FILE=terraform_${TERRAFORM_VERSION}_linux_amd64.zip
RUN wget -q https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/${TERRAFORM_FILE} \
    && unzip ${TERRAFORM_FILE} -d /usr/local/bin \
    && rm -rf ${TERRAFORM_FILE}


# Install Amazon AWS-ClI v2
RUN     curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"; \
        unzip awscliv2.zip; \
        ./aws/install --update; \
        rm -rf ./aws awscliv2.zip;

# Install Kubectl
ARG KUBECTL_VERSION=v1.17.3
RUN wget -q https://storage.googleapis.com/kubernetes-release/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl \
    --directory-prefix=/usr/local/bin/ \
    && chmod a+x /usr/local/bin/kubectl 

RUN echo $AWS_ACCESS_KEY_ID

ENV HOME=/tmp`

Create Container 

`docker build -t tf12.31 .`

*Container Run*
`docker run -d -ti -p 80:80 \
 -w /data \
 -v $PWD:/data:rw,z \
 -e AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} \
 -e AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} \
 -e AWS_SESSION_TOKEN=${AWS_SESSION_TOKEN} \
  tf12.31`
  
*Container login*
`docker exec -it -e AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} -e AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} -e AWS_SESSION_TOKEN=${AWS_SESSION_TOKEN} IDCONTAINER /bin/sh`
