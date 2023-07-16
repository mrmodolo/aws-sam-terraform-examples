# AWS SAM and Terraform

This repository contains a sample book reviews serverless application written in Terraform. 
Full instructions on how to use this repository are contained in blog post [Better Together: AWS SAM CLI and Hashicorp Terraform](https://aws.amazon.com/blogs/compute/better-together-aws-sam-cli-and-hashicorp-terraform/)

## Install AWS SAM CLI

cd ~/Downloads
wget https://github.com/aws/aws-sam-cli/releases/latest/download/aws-sam-cli-linux-x86_64.zip
unzip aws-sam-cli-linux-x86_64.zip -d sam-installation
sudo ./sam-installation/install --update
rm aws-sam-cli-linux-x86_64.zip*
rm -rf sam-installation
sam --version
SAM CLI, version 1.90.0

