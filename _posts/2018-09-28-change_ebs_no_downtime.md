---
title: Change EBS volume size without downtime
key: 20180928
tags: aws ebs ec2 linux
---

## Part 1. AWS Console

Navigate to ec2 → ebs volume → volume that needs modification.   
Click `Modify Volume` and change size to desired one and wait until it's finished.  
![ebs_modify_volume]({{site.baseurl}}/assets/images/posts/modify_ebs_volume.png){:class="img-responsive"}


## Part 2. Modify volume inside ec2 instance

Instructions can also be found [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/recognize-expanded-volume-linux.html).

Basically use `lsblk` to check if partition uses the whole device size
```bash
$ lsblk
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
xvda 202:0 0 16G 0 disk
└─xvda1 202:1 0 8G 0 part
```
if not, (like above) use `sudo growpart /dev/xvda 1`  (arguments match partition/device) so that partition can be expanded to device size.

Then using `df -h` you might still see that os doesn't see above change. At this point use something like `sudo resize2fs /dev/xvda1`

After `resize2fs`, check again `df -h` command, result should show the new volume size.

**HINT:** If root disk space has ran out and you are trying to resize, `growpart` command might not work due to lack of disk space, at this point, simply delete something with no value just to give shell some space to run the command.