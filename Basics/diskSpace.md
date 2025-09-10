## Disk space issue in general and for Jenkins
* Jenkins consume the storage as the plugins are installed and also the data added in the workspcae by the jobs
* Increase the volume size of jenkins server in AWS and reeboot the ec2 instance in aws console

* Run the commands before and after increasing the volume size to see the increased size

1. list of Partion
```
df -h
```

2. list of disks and partions of respective disks
```
fdisk -l
```