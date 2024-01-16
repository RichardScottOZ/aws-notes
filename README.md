# AWS NOTES

## ec2
### Windows
- Git Bash is your friend!
- unixutils latest is good too 
	- https://sourceforge.net/projects/unxutils/
	
### Ubuntu
- e.g. don't use these
	- GDAL on Amazon Linux 2
	- https://gist.github.com/abelcallejo/e75eb93d73db6f163b076d0232fc7d7e
	- https://stackoverflow.com/questions/36772111/how-can-i-install-a-recent-version-of-gdal-on-amazon-linux/44907360#44907360
	

### Drive Mounting
- examples
```bash
lsblk

sudo mkfs -t ext4 /dev/xvdb
sudo mkfs -t ext4 /dev/nvme1n1

mkdir data

sudo mount /dev/xvdb data
sudo mount /dev/nvme1n1 data

- if already used can be a partition number e.g. /dev/nvme1n1p1 etc.

sudo chmod 777 data
cd data
```

### Increasing Drive Size
- after adjusting the volume, things like this - depending on filesystem ext4, xfs etc
```bash
sudo growpart /dev/xvda 1
CHANGED: partition=1 start=2048 old: size=16775135 end=16777183 new: size=20969439,end=20971487
We can confirm that all the storage is allocated to xvda1 by listing block storages again:

~$ sudo lsblk
NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  10G  0 disk
└─xvda1 202:1    0  10G  0 part /
We increased block storage, but we also need to increase filesystem:

~$ sudo resize2fs /dev/xvda1
```

or

```
gdisk
parted

check filetype sudo file -s /dev/nvme1n1
sudo xfs_growfs -d /data

growpart /dev/nvme0n1

resize2fs /dev/nvme0n1p1
resize2fs /dev/nvme0n1

growpart /dev/nvme1n1

resize2fs /dev/nvme1n1p1
```

- root volume size
- https://www.dolthub.com/blog/2022-05-02-use-more-than-2TB-ubuntu-ec2/


### Instances
```bash
aws ec2 start-instances --instance-ids i-05a13c4f0164a6168
aws ec2 stop-instances --instance-ids i-05a13c4f0164a6168
```

### Connect
```bash
ssh -i robotspacezombie.pem ubuntu@44.444.4.4
111

### Keep Yourself Alive
- Don't want your sessions to do when you are cut off
- Use screen or something similar
- https://stackoverflow.com/questions/32500498/how-to-make-a-process-run-on-aws-ec2-even-after-closing-the-local-machine

### Secure Copy
```bash
$ scp -i dumbkeyfilename.pem ubuntu@44.444.44.44:/home/ubuntu/annoying.yaml . annoying.yaml  

scp -i yaytest.pem ubuntu@10.888.8.88:/tmp/boringserver/tediouslogfile.tar.gz   .
```

### Jupyter
- Along these lines
	- https://docs.aws.amazon.com/dlami/latest/devguide/setup-jupyter.html
- connecting to ubuntu ec2 for jupyter book - use right domain name - also use 127.0.0.1 not localhost as that fails
- start jupyter in a session [after working out what ports you want re: above]
- git bash
```bash
ssh -i giantrobotdinosaur.pem -N -f -L 9992:127.0.0.1:8888 ubuntu@ec2-44-44-44-234.us-west-2.compute.amazonaws.com
```

### Batch

- EC2 Batch GPU Cloudformation - https://github.com/johnewart/ec2-batch-gpu-cloudformation/blob/master/gpu-ami-batch.yaml

- Creating a Compute Resource - https://docs.aws.amazon.com/batch/latest/userguide/create-batch-ami.html

- Get GPU AMI list - https://docs.aws.amazon.com/batch/latest/userguide/batch-gpu-ami.html
- https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-ami-versions.html#ecs-ami-versions-linux

#### AWS Deep Learning Containers
- https://github.com/aws/deep-learning-containers/blob/master/available_images.md
- 7673104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-training:1.15.5-cpu-py36-ubuntu18.04

AWS Deep Learning Container login
https://docs.aws.amazon.com/deep-learning-containers/latest/devguide/deep-learning-containers-custom-images.html


## s3
### cli key 
```bash
AWS_ACCESS_KEY_ID=AAAAAAAAAAAAAAAAAAAA AWS_SECRET_ACCESS_KEY=AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA aws s3 ls
```

### configuration
```
aws configure set default.s3.max_concurrent_requests 1000
aws configure set default.s3.max_queue_size 10000
aws configure set default.s3.max_queue_size 100000
```

### data
```bash
- default pattern : exclude everything wildcard, include what you want
- IT IS A VERY GOOD IDEA TO CHECK DELETIONS FIRST!
aws s3 rm "s3://hopethesearenotimportant/rasters/" --recursive --exclude="*" --include="*banana.tif" --dryrun
(dryrun) delete: s3://hopethesearenotimportant/rasters/GIANT_MEGA_SUPER_GODZILLA_banana.tif

aws s3 rm "s3://hopethesearenotimportant/rasters/" --recursive --exclude="*" --include="banana"
```

### bucket permissions
```python
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::rainbow-unicorn"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::rainbow-unicorn/*"
            ]
        }
    ]
}
```

### launch template type permissions
-  https://medium.com/@rajganeshp/aws-lab-ec2-access-to-s3-using-iam-role-cedefbbbc118
-  add for role and IAM:PassRole for user too
-  
