# AWS

## Install AWSCLI

````bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
````

## Install Session Manager Plugin

The Session Manager Plugin is an extension for the AWS CLI that allows you to open interactive shell sessions (like SSH) into your EC2 instances using AWS Systems Manager (SSM) — without needing:

- SSH key pairs
- Open ports (like port 22)
- Bastion hosts

It works through AWS APIs and is secured/audited via IAM, CloudTrail, and optionally CloudWatch or S3.

````bash
curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb" -o "session-manager-plugin.deb"
sudo dpkg -i session-manager-plugin.deb
````

## Config AWSCLI

### ~/.aws/credentials
This file stores the actual AWS credentials (Access Key ID and Secret Access Key) for one or more named profiles.
Example:
````ini 
[default]
aws_access_key_id = AKIAEXAMPLE
aws_secret_access_key = abc123examplekey

[dev]
aws_access_key_id = AKIADEVKEY
aws_secret_access_key = devsecretkey
````

