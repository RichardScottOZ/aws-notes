# AWS NOTES

## ec2
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