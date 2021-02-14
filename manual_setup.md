
# Manually Setting up a Cloud Computer to Run PixInsight

## Prerequisites

In order to set the cloud computer up the following prerequisites are needed:

* An AWS account, see `<xxx>` how to set it up
* PixInsight Linux binaries downloaded to your computer
* A PixInsight license

## Overview

to be written. `<xxx>`

## Step 1 - Setting up your AWS account

We will use an AMI (Amazon Machine Image) provided by a third party to setup the server in the cloud. 
In order to be able to use the given AMI, you need to subscribe to the given 3rd party's offering in the Amazon Marketplace.
So open up the AWS Marketplace an search for `Nice DCV`. At the time of writing there's only one offering. Read through the 
conditions and agree to the costs (a few cents per compute hour). 

## Step 2 - Launching an instance

Return to the AWS console, switch to the EC2 (Elastic Compute Cloud) console and press the `Launch an instance` button.
When asked to select an AMI, again search for `Nice DCV`, click on "Show Marketplace Results" in the initial results and 
choose the appropriate AMI, which is `Ubuntu 18 LTS (no GPU)`. 

When asked for the instance type, choose e.g. t3.xlarge, giving you 4 vCPUs, 16 GiB of RAM (at the time of writing). The larger 
the instance type, the more expensive the compute hour will be.

In the steps that follow:
- increase the default volume size of 8 GB to 10 GB and create a second volume with a size of ~250 GB.
- Attach a security policy allowing SSH and DCV. 

## Step 4 - Launch the instance and attach the right IAM role

Launch the instance and attach an IAM role to the running instance, that allows access to the DCV license,
see [here](https://docs.aws.amazon.com/dcv/latest/adminguide/setting-up-license.html) 

## Step 5 - Prepping the instance

Use SSH to login to the instance. **First Create a DCV session** suitable for auto sizing to your display:

```bash
$ sudo dcv close-session console
$ dcv create-session mysession
```

The DCV console session is bound to the physical display of the server, which might be limited in size depending 
on the instance type you have chosen. The two commands above close the console session and create a virtual session, 
which basically is unlimited in size and will autosize to the client's display, once you connect with the DCV client (see below)

**Second format and mount the second volume:** Look which device name was assigned to the second volume using `lsblk`
Then format and mount that device and prepare some directories when mounted.

```bash
$ lsblk
$ sudo mkfs -t ext4 /dev/<device gleaned from lsblk output>
$ sudo mount /dev/<device> /mnt
$ sudo mkdir /mnt/data   # This is where data will go
$ sudo chown ubuntu:ubuntu /mnt/data
$ sudo mkdir /mnt/tmp    # Scratch directory for PixInsight
$ sudo chown 1777 /mnt/tmp
```

**Third install PixInsight** as per description [in a Forum post here](https://pixinsight.com/forum/index.php?threads/how-to-install-pixinsight-on-linux.14324/) or as written in the 
description, when downloading the PixInsight Linux binaries from the distribution site.

Also **set a password for the `ubuntu` user**:

```bash
$ sudo passwd ubuntu
```

## Step 5 - Installing a native DCV client on your local machine

Install [NICE DCV Client](https://download.nice-dcv.com/) on your local computer, start the client and connect to the server. 
You need user `ubuntu` and password as set in the previous step. You can copy the public IP4 address from the AWS console.

## Step 6 - Activate & Configure PixInsight

Either copy over your given license as described in the [PixInsight FAQs](https://pixinsight.com/faq/) (see 2.7 and 2.10), 
or start and reactivate the copy of PixInsight on the server.

First thing after activating PixInsight, is to open up the preferences process and set the `tmp` directory to `/mnt/tmp`. 
On that occasion also change the folder PixInsight downloads to `/mnt/tmp`.

## Step 7 - Download your astropix to the instance

Install the AWS CLI on the server, configure it and download your pics.

```bash
$ sudo apt install awscli
$ aws configure
$ cd /mnt/data
$ aws s3 sync s3://<your bucket>/<your project>/ . 
```

## Step 8 - Start your PixInsight processing

Then fire away with your astro processing.

# Saving Costs between Editing Sessions

Once you are finished with your processing, you can shutdown the instance in the AWS management console.
You will notice that most of the cost is spent on the harddisk, that is attached to the instance. In order to save
costs between editing sessions, you can remove the disk in between sessions. Do the following:

## Store your work

First make a backup of your precious work results. 
Using the aws cli, store your work into a S3-bucket

```bash
$ cd PI 
$ aws s3 sync . s3://<your bucket>/<your project>/<results>
```

## Shutdown and Detach

In the management console, shutdown the EC2 instance and then detach the volume, then delete it. 
Note that this cannot be undone and you have to create a new volume and attach it to the computer before you start it up. 
You will have to format and mount the volume after startup.
