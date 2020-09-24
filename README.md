# cloud-pi
Setup a AWS cloud instance of PixInsight automatically.

As cloud computing is more and more ubiquitous, it is now much, much cheaper to have a decent cloud instance running PixInsight, 
than buying and only sporadically using a decently sized desktop computer. Basically turning a large upfront investment into many
small payments (pay per use).

The scripts in this project (to be created) are intended to make setup of an AWS instance running PixInsight almost fully automatic.

# Risks & Caveats of using a server in the cloud
A server and especially a large server in the cloud can cost a fortune, if left running all the time and going undetected. 
So make sure that you switch it off after use. A misconfigured S3-bucket can also incur download charges. 
The authors of this documentation are not responsible for any of those charges and provide no liability for cloud use. 
Use at your own risks.

# Status of this project
Initial proof of concept setup is done, i.e. a server has been manually setup and PixInsight runs on it.
See [Manual Setup](manual_setup.md) for how to recreate that. Some configuration quirks still exist. 
Once these quirks have been removed, I will start automating and recreating the setup from scratch.

# Software used
The software used on the cloud computer to provide a seamless cloud experience of PixInsight are:
* Ubuntu, installing the default GUI desktop environment in the cloud
* NICE DCV, streaming the desktop to a client running on your laptop or desktop (like Google Stadia)
* PixInsight, for sure. :-)

The setup assumes that you have uploaded your images into an S3-bucket, e.g. using the aws command line interface, see 
[Uploading Images to S3](upload.md)


