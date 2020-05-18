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
3.) Set-up S3

To allow files to be uploaded to Amazon S3, it is required to create S3 bucket in West-2 region. Create the following buckets on S3. <br/>
- indeesfxsync 
- indeefdata 
- indeednfsworkdir 
 
In the bucket policy, make sure add the following policy:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": "arn:aws:s3:::indeenfsworkdir/sim18-run-batch/output/*"
        }
    ]
}
```

In the CORS configuration, make sure add the following xml code:
```
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>yourdomain.com</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>POST</AllowedMethod>
    <AllowedMethod>PUT</AllowedMethod>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```

To download test case from the existing (old) account, please follow the instruction below: <br/>
- Download awscli on local pc/laptop. For Linux, use the following commands <br/>
```
sudo -H pip install awscli
```
- assuming aws is already configured to the exiting (old) account, then issue the following command to download the test case <br/>
```
aws s3 sync s3://indeenfsworkdir/sim22-run-batch/potential .
```

In the new account, just drag and drop and test case folders and files to the S3 bucket. In this case s3://indeenfsworkdir/sim22-run-batch/

4.) Set-up AWS Batch

(in progress)

- request for limit increase t0 1027 for on-demand instances 
