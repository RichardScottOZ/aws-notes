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
- cli key AWS_ACCESS_KEY_ID=AAAAAAAAAAAAAAAAAAAA AWS_SECRET_ACCESS_KEY=AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA aws s3 ls