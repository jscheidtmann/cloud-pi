
# Using an S3-bucket to store astronomy images in the Cloud

## Install the AWS CLI (Command Line Interface)

You can find download and install instructions [here][https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html].

Once installed on your computer, configure it: 

```bash
$ aws configure
AWS Access Key ID [****************]:
AWS Secret Access Key [****************]:
Default region name [eu-central-1]:
Default output format [None]:
```

Supply the Access Key ID and Secret of an account or user, that has been configured to be able to access 
and use S3 (Simple Storage Service). As Default region, use the one that gives 
the lowest [Cloud Ping][https://www.cloudping.info/]. 

## Storage location
Now you can list your S3 buckets: 

```bash
$ aws s3 ls
```

or create a new bucket (mb = make bucket):

```bash
$ aws s3 mb s3://<your bucket name>
```

## Copy your images to the cloudping

Now you can change to the directory where your pictures reside and copy the respective folder into the bucket:

```bash
$ cd <where your images live>
$ aws s3 sync . s3://<your bucket name>/<give a foldername for your astropic>
```

This will copy the contents of the current directory (indicated by `.`) to the cloud. 

## Copy your images back to a computer

Copying back is achieved either by syncing or copying. Note that this may incur data transfer charges.

```bash
$ ssh <to some computer>
remote$ cd <somewhere on your remote server> 
remote$ aws s3 ls s3://<your bucket name>/<foldernames>  # List contents
remote$ aws s3 cp s3://<your bucket name>/<foldername>/ . --recursive
```

