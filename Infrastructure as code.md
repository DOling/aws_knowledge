**TOC**

1. [[#Configuring AWS with Terraform|Configuring AWS with Terraform]]
	1. [[#Tools|Tools]]
		1. [[#LocalStack|LocalStack]]
			1. [[#Use cases|Use cases]]
			1. [[#Application settings|Application settings]]
				1. [[#Environment variables|Environment variables]]
				1. [[#Development with #docker-compose|Development with #docker-compose]]

# Configuring AWS with Terraform
## Tools
### LocalStack
* [LocalStack](https://github.com/localstack/localstack) replicates a cloud in a local Environment
	* allows testing locally
	* is a heavy dependency --> slows test speed down
#### Use cases
* writing tests against LocalStack resources
* end-to-end test of #serverless applications
	* deploy AWS CloudFormation template to LocalStack and run tests
* validating #InfrastructureAsCode (IAC)

#### Application settings
##### Environment variables
You can pass a bunch of environment variables to #LocalStack to configure it. The most important ones are listed below. [Here](https://github.com/localstack/localstack#configuration) you will find a list of core configurations.
* **EDGE_PORT**
	* LocalStack port that listens for AWS API requests
	* default: **4566**
	* change this port for each of your project if you are running LocalStack in a bunch of different projects
* **SERVICES**
	* list of services names to start when LocalStack first launches
		* service names correspond to the names available in the [AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/)
		* example: ```"SERVICES=kinesis,lambda,s3"```
* **DEFAULT_REGION**
	* region to use when talking to the API
	* default: **us-east-1**
* **AWS SDKs**
	* use inside LocalStack container
	* AWS_DEFAULT_REGION
	* AWS_ACCESS_KEY_ID
	* AWS_SECRET_ACCESS_KEY
* **HOSTNAME**
	* name of the host to expose the services internally
	* ensure that traffic is routed correctly
* **HOSTNAME_EXTERNAL**	
	* name of the host to expose the service externally
	* ensure that traffic is routed correctly
* **DATA_DIR**
	* persist LocalStack data across sessions
		* **requires Docker volume mount**
		* **DATA_DIR**
			* directory inside container where data is stored
		* supported AWS services
			* Kinesis
			* DynamoDB
			* Elasticsearch
			* S3
			* Secrets Manager
			* SSM
			* SQS
			* SNS
* **INIT_SCRIPTS_PATH**
	* create AWS resources required by your application
		* only run if no resources are found in LocalStack
		* scripts in path are run when container is first created
	* **requires docker volume mount**
	* 
##### Development with #docker-compose
* configuration is self-documenting and reproducible
* **requires ```docker-compose``` version 1.9.0+**
* **NOTE:** On MacOS you may have to run ```TMPDIR=/private$TMPDIR docker-compose up``` if ```$TMPDIR``` contains a symbolic link that cannot be mounted by #Docker
