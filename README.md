# cloud-pi
Setup a AWS cloud instance of PixInsight using Terraform

As cloud computing is more and more ubiquitous, it is now much, much cheaper to have a decent cloud instance running pixinsight, 
than buying and only sporadically using a decently sized desktop computer. 

The scripts in this project are intended to make setup of an AWS instance running PixInsight almost fully automatic.

# Software used
The software used on the cloud computer to provide a seamless cloud experience of PixInsight are:
* Ubuntu, installing the default GUI desktop environment in the cloud
* NICE DCV, streaming the desktop to a client running on your laptop or desktop (like Google Stadia)
* PixInsight, for sure. :-)

# Prerequisites

In order to set the cloud computer up the following prerequisites are needed:

* An AWS account, see ... how to set it up
* Terraform, installed on your computer for setting up the cloud computer and executing all the necessary steps 
* PixInsight Ubuntu binaries downloaded to your computer
* A PixInsight license

Your PixInsight license will be copied over to the Linux instance running in the cloud.

# Status of this project
Initial development ongoing.
