# packer-virtualbox-ami-centos7

This [Packer](https://packer.io/) template will built an extremely bare [CentOS 7](https://www.centos.org/) image from the NetInstall ISO, intended for deployment to [EC2](https://aws.amazon.com/ec2/).

This image conforms to Amazon's [guidelines](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/building-shared-amis.html), and uses [cloud init](https://cloudinit.readthedocs.org) to provision the image upon deployment.

Follow Amazon's [import/export guide](https://aws.amazon.com/ec2/vm-import/) to learn how to deploy the OVA once created.

# Usage

## Building
To use a different kickstart script, just set the `profile` accordingly.
```
packer build -var 'profile=ext4' template.json
```

## Uploading to S3
```
aws s3 cp output-ext4/centos7-ec2-ext4.ova s3://my-ami-bucket/centos7-ec2-ext4.ova
```

## Importing to EC2
```
aws ec2 import-image --cli-input-json '{  "Description": "CentOS 7 EXT4", "DiskContainers": [ { "Description": "CentOS 7 EXT4", "UserBucket": { "S3Bucket": "my-ami-bucket", "S3Key" : "centos7-ec2-ext4.ova" } } ]}'
```
