# AWS-Batch-Migration
User Account Migration Step-by-Step Guide

## Requirements:
Ensure new user account has the following permissions:
- CloudFormation
- ECS
- EC2
- S3 

## Steps:
1.) click on launch/play button to start install the netflow template from AWS site using CloudFormation service<br/>
https://docs.opendata.aws/genomics-workflows/orchestration/nextflow/nextflow-overview/

Make sure to setup same parameters as old account

2.) Install dockerfile using Cloud9 service

2-1) Choose AWS Linux operating system <br/>
2-2) Modify volume to 20GB <br/>
2-3) Set-up both Dockerfile and cfdrun.sh in containers folder (see below) and issues the following docker commands
```
cd openfoam-cfd/containers
$(aws ecr get-login --no-include-email --region us-west-2)
docker build -t nextflow .
docker tag nextflow:latest 369256576344.dkr.ecr.us-west-2.amazonaws.com/nextflow:latest
docker push 369256576344.dkr.ecr.us-west-2.amazonaws.com/nextflow:latest
```
