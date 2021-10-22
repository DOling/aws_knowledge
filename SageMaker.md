**TOC**

1. [[#SageMaker Studio Notebooks|SageMaker Studio Notebooks]]
	1. [[#Overview|Overview]]
	1. [[#Capabilities|Capabilities]]
1. [[#SageMaker notebook instances|SageMaker notebook instances]]
	1. [[#Customize notebook with [_Lifecycle Configuration_][1] script|Customize notebook with [_Lifecycle Configuration_][1] script]]
		1. [[#Create a _Lifecycle Configuration_|Create a _Lifecycle Configuration_]]
			1. [[#Option 1|Option 1]]
			1. [[#Option 2|Option 2]]
			1. [[#Time out|Time out]]
	1. [[#_Lifecycle configurations_ to control internet access|_Lifecycle configurations_ to control internet access]]
1. [[#References|References]]

# SageMaker Studio Notebooks
## Overview
* AWS SM Studio is an IDE living in the cloud
* Studio notebooks is a combination of instances & #AWS/SageMaker images:
	* Instance type: hardware configuration the notebooks run on
		* Amount of memory
		* Determine pricing & costing
	* Studio notebooks consists of SageMaker images
		* Combination of language packages
			* Libraries
			* Frameworks
			* Binaries
		* Kernel
			* Conda environment
## Capabilities
* Experience the same features of a #Jupyter/notebook and a Jupyter Lab
* viewing the notebooks --> **no cost** (Status: #InService)
* starting the kernel produces cost --> execution of a #Jupyter/notebook
* 1-to-1 maping between user and instances
	* collaborate with colleagues using snapshots
		* share a read-only copy of your notebook using a #hyperlink
		* editable by creating a copy
* provide persistent custom development environments
	* #build custom #container with approved base images
	* push to docker registry/ #AWS/ECR
	* attach kernel to Studio domain
	* select kernel to run notebook
* attach an #AWS/EBS volume to the SageMaker Notebook Instance
	* choose any size between 5 GB and 16384 GB, in 1 GB increments; 5 GB is the default value
	* volume size can be increased by updating the notebook Instance
	* **only files and data saved within the ```/home/ec2-user/SageMaker``` directory persist between notebook instance sessions**
	* each notebook instance's ```/tmp``` directory provides a minimum of 10 GB of storage in an instance store; **instance store is temporary, block-level storage that isn't persistent; SageMaker deletes the directory's contents when the instance is stopped or restarted**

# SageMaker notebook instances
*  #AWS/SageMaker/notebook instance & SageMaker Studio are interfaces to AWS SageMaker service
	*  creating a SM notebook instance builds an Amazon Elastic Inference connected to an #AWS/EC2 instance with access to an #AWS/EBS (Amazon Elastic Block Storage)
	*  ability to build persistent python environments
* #AWS/SageMaker/notebook instance environment
	* Jupyter Kernel
	* Python packages
	* installation of external libraries and kernels possible

# Create a Notebook Instance
https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-create-ws.html

## Customize notebook with [_Lifecycle Configuration_][1] script
The following are best practices for using lifecycle configurations:
* Lifecycle configurations run as the `root` user. If your script makes any changes within the `/home/ec2-user/SageMaker` directory, (for example, installing a package with `pip`), use the command `sudo -u ec2-user` to run as the `ec2-user` user. This is the same user that Amazon SageMaker runs as.
* SageMaker notebook instances use `conda` environments to implement different kernels for Jupyter notebooks. If you want to install packages that are available to one or more #Jupyter/notebook kernels, enclose the commands to install the packages with `conda` environment commands that activate the conda environment that contains the kernel where you want to install the packages.
* #AWS/SageMaker/notebook instances have direct internet access by default
	* ability to download packages, notebooks and datasets
	* access other SageMaker components through the **public** internet

### Create a _Lifecycle Configuration_
**[Lifecycle configuration best practices][2] is a good starting point!**
#AWS/SageMaker/notebook/lifecycle_configuration
#### Option 1
Create a _Lifecycle Configuration_ during the creation of a new SageMaker notebook instance.
The following blog entry will guide through the steps: [# Customize your Amazon SageMaker notebook instances with lifecycle configurations and the option to disable internet access](https://aws.amazon.com/blogs/machine-learning/customize-your-amazon-sagemaker-notebook-instances-with-lifecycle-configurations-and-the-option-to-disable-internet-access/)
#### Option 2
Create a permanent _Lifecycle Configuration_ that can be used by new SageMaker notebook instances.
To do this, open the SageMaker console and navigate to `Notebook -> Lifecycle configurations`. 
Here you can define two kinds of scripts:
1. `on-create`:
	* install the `ipykernel` library to create custom environments as Jupyter kernels
	* use `pip install` and `conda install` to install libraries
2. `on-start`
	* install any custom environment that you create as Jupyter kernels, so that they appear in the dropdown list in the **Jupyter New** menu

#### Time out
The setup of a SageMaker notebook instance with a _Lifecycle configuration_ times out after 5 minutes. This [link][3] describes two options for a resolution.
Reasons for time outs:
* SageMaker Notebook can not reach the public internet; can be quickly evaluated with a `curl` command in the `on-create` script: 
``` bash
$ curl -I https://wikipedia.org
HTTP/1.1 200 OK
```

**Comment**: SageMaker does not update the libraries installed with the _Lifecycle configurations_. SageMaker conserves the versions of the libraries, so that the custom environment has always the expected specific versions of libraries that you want.

## _Lifecycle configurations_ to control internet access
_Lifecycle configurations_ allows to disconnect the notebook instances from the public internet so that controlled security settings can be applied in the notebook instances.

The following [link][4] describes some options to achieve that.

# References
[1]: https://docs.aws.amazon.com/sagemaker/latest/dg/notebook-lifecycle-config.html
[2]: https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-lifecycle-config-install.html
[3]: https://aws.amazon.com/premiumsupport/knowledge-center/sagemaker-lifecycle-script-timeout/
[4]: https://aws.amazon.com/blogs/machine-learning/customize-your-amazon-sagemaker-notebook-instances-with-lifecycle-configurations-and-the-option-to-disable-internet-access/
1. [Install external libraries and kernels in notebook instances](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-add-external.html)
2. [Customize a notebook instance using a lifecycle configuration script](https://docs.aws.amazon.com/sagemaker/latest/dg/notebook-lifecycle-config.html)