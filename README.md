# AWS NOTES

## ec2
### Windows
- Git Bash is your friend!

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

### Instances
```bash
aws ec2 start-instances --instance-ids i-05a13c4f0164a6168
aws ec2 stop-instances --instance-ids i-05a13c4f0164a6168
```

### Secure Copy
```bash
$ scp -i dumbkeyfilename.pem ubuntu@13.239.47.46:/home/ubuntu/annoying.yaml . annoying.yaml  

scp -i yaytest.pem ubuntu@10.888.8.88:/tmp/boringserver/tediouslogfile.tar.gz   .
```

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