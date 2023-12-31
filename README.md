# AWS SAM and Terraform

This repository contains a sample book reviews serverless application written in Terraform. 
Full instructions on how to use this repository are contained in blog post [Better Together: AWS SAM CLI and Hashicorp Terraform](https://aws.amazon.com/blogs/compute/better-together-aws-sam-cli-and-hashicorp-terraform/)

## Install AWS SAM CLI

```bash
cd ~/Downloads
wget https://github.com/aws/aws-sam-cli/releases/latest/download/aws-sam-cli-linux-x86_64.zip
unzip aws-sam-cli-linux-x86_64.zip -d sam-installation
sudo ./sam-installation/install --update
rm aws-sam-cli-linux-x86_64.zip*
rm -rf sam-installation
sam --version
SAM CLI, version 1.90.0
```

## Error: a bytes-like object is required, not 'str'

After running the example successfully the first time, I started getting the error after further attempts.
Searching on google I found:

- [Bug: sam local invoke - Error: a bytes-like object is required, not 'str' on decompress #5434](https://github.com/aws/aws-sam-cli/issues/5434)
- [SAM INVOKE FAILS #5421](https://github.com/aws/aws-sam-cli/issues/5421)

The solution was to run the command `sam local invoke` with `--skip-pull-image` 

```bash
sam local invoke --skip-pull-image aws_lambda_function.publish_book_review -e events/new-review.json --beta-features 
```

## Podman [TODO]

[AWS SAM and Podman](https://www.reddit.com/r/podman/comments/r6ybkw/aws_sam_and_podman/)

> systemctl --user enable --now podman.socket
>
> systemctl --user start podman.socket
>
> systemctl --user status podman.socket
> 
> podman create --name="docker-cli" docker:dind
>
> podman cp docker-cli:/usr/local/bin/docker ./docker
>
> podman rm docker-cli
> 
>
> unalias docker
>
> export DOCKER_HOST=unix:///run/user/$UID/podman/podman.sock
>
> ./docker --version


```bash
systemctl --user enable --now podman.socket
systemctl --user start podman.socket
systemctl --user status podman.socket

podman create --name="docker-cli" docker:dind
podman cp docker-cli:/usr/local/bin/docker ~/bin/docker
podman rm docker-cli

sudo touch /etc/containers/nodocker # supress messages

export DOCKER_HOST=unix:///run/user/$UID/podman/podman.sock

docker --version
```

# Remove Resources

```
aws dynamodb delete-table --table-name BookReviews
aws logs delete-log-group --log-group-name /aws/api_gw/book_reviews_service

aws iam list-role-policies --role-name iam_for_lambda
aws iam delete-role-policy --role-name iam_for_lambda --policy-name dynamodb_access
aws iam delete-role --role-name iam_for_lambda

aws lambda delete-function --function-name publish-book-review
```

## AWS Lambda Terraform module

[AWS Lambda Terraform module](https://registry.terraform.io/modules/terraform-aws-modules/lambda/aws/latest)

Terraform module, which takes care of a lot of AWS Lambda/serverless tasks (build dependencies, packages, updates, deployments) in countless combinations

