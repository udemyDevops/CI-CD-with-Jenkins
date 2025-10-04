
## Docker CI flow
* [_Prerequisites for docker PaaC_](#prerequisites-for-docker-paac)
* [_Steps for docker CI in Jenkins](#steps-for-docker-ci-in-jenkins)
    - [_Installing docker engine and aws cli in jenkins server_](#install-docker-engine-and-aws-cli)
    - [_Creating IAM user and ECR in AWS_](#creating-iam-user-and-ecr-in-aws)
    - [_Installing plugins and storing credentials in jenkins_](#install-the-plugins-and-store-credentials-in-jenkins)
* [_Pipeline for docker CI_](#pipeline-for-docker-ci)

![alt text](Docker-CI-flow.png)

## Prerequisites for docker PaaC
* AWS
    - IAM user with Access keys
    - ECR (container registry)
* Plugins
    - ecr
    - docker
    - docker pipeline
    > docker plugins are used to run the docker commands

* Jenkins
    - store aws access keys (credentials)
    - Install docker engine in jenkins
        - Add jenkins user to docker group and reeboot --> jenkins user will execute docker commands and should have that privilege
        > To run the docker commands we need the docker engine

### Steps for docker CI in Jenkins
* Install docker engine in jenkins
    - Add jenkins user to docker group and reboot --> jenkins user will execute docker commands and should have that privilege
* Install AWS CLI --> required for CD to deploy the docker image (artifact)
* Create IAM user in AWS to use access and secret keys
* Create ECR (container registry)
* Plugins
    - ecr
    - docker pipeline
    - aws SDK for credentials
        > when we install ecr plugin it will also install aws sdk but if doesn't then we need to install it. This will be used to save the access and secret keys in jenkins
* Store the AWS credentials (Access and secret keys of IAM user) in Jenkins
* Run the pipeline

#### Install docker engine and aws CLI
* ssh to Jenkins server (since we used ubuntu for jenkins, username would be ubuntu in aws ec2)
```
ssh -i <key path> ubuntu@<jenkins server ip>
```
```
sudo apt update
```
* Install AWS-CLI
```
sudo snap install aws-cli --classic
```
* switch to root user
```
sudo -i
```
* Install docker engine (https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```
> Check the status
```
systemctl status docker
```
* Now add jenkins user to docker group (when docker is installed it also create docker group)
```
usermod -a -G docker jenkins
```
> to check if it is added
```
id jenkins
```
* Reboot the jenkins server to load the configurations and changes
```
reboot
```

#### Creating IAM user and ECR in AWS
* In AWS --> IAM --> users --> click on `create user`
    - give a name like 'jenkins' --> Next
    - permission options --> select `Attach policy directly`
        - permission policies --> Add the policies `AmazonEC2ContainerRegistryFullAccess` and `AmazonECS_FullAccess`
        - Next
        > This user used by jenkins require access to ECR to upload the docker image and also to ECS (container service) to deploy the image in CD pipeline
    - Create user
    - Select the created user --> security credentials --> create access key --> for use case select 'CLI' --> tick the confirmation box and click on 'Next' --> click on `create access key`
        > Copy the access and secret keys or donwload the csv file

* In AWS --> ECR --> create --> give a name and click on `create`

#### Install the plugins and store credentials in jenkins
* To install the plugins
    -   In Jenkins Dashboard --> manage jenkins --> plugins --> available plugins
        - search for 'aws' --> select `Amazon Web Services SDK::All`
        - search for 'ecr' --> select `Amazon ECR`
        - search for 'docker' --> select `Docker Pipeline` and `CloudBees Docker Build and Publish`
        - click on `Install`

* To store the credentials for AWS
    - Jenkins Dashboard --> manage jenkins --> credentials --> under `stores scoped to jenkins` click on `system` --> Global credentials (unrestriced) --> click on `Add credentials`
        - For `kind`, select `AWS Credentials` (this option is visible after installing the AWS SDK plugin)
        - Scope --> Global
        - Id --> giva a name to identify with AWS credentials (this name will be used in pipeline code)
        - add Description
        - Access key ID --> copy the access key ID from the CSV
        - Secret Access key --> copy the secret access key from the CSV
        - click on `create`


### Pipeline for Docker CI
* update the values in [_Jenkinsfile for docker CI_](Jenkinsfile-for-docker-CI)

* In Jenkins --> `+ New Item` --> Give a name and select item type as `pipeline` and click ok
    - Under Pipeline --> Definition --> Pipeline script --> paste the pipeline script
    - save --> build now

    > Once the job is completed, can see the image in ECR.
    > The image is uploaded to ECR but also exist in Jenkins, so we need to another stage to remove the image from Jenkins after the upload to ECR is completed (covered in the pipeline code)

### Pipeline for Docker CICD
![alt text](Docker-CICD-flow.png)

* It is an extension of CI pipeline to also include the delivery of code (deployment) to the Container hosting platform
    - Docker engine (local development / testing -- install docker engine and run containers using docker commands, No HA, self-healing, etc. required to run in Stage and prod environment)
    - Kubernetes
    - ECS (Elastic Container Service - for this exercise)

* Prerequisites:
    - ECS cluster and service
    - Plugin --> pipeline: aws steps

#### Creating ECS cluster and ECS service in AWS
* In AWS --> search for ECS
    - click on `Clusters` --> click on 'create cluster'
        - give a name
        - under Networking --> vpc and subnets (of all zones available in the region are selected), leave the default selection or choose a custom vpc and subnets
        - under Infrastructure --> AWS fargate (serverless) - default
            > can also use ec2 instances
        - Monitoring (optional) --> enable container insights (if required)
        - tags (optional) --> add the tags related to the project
        - click on `create`
            > If any error after clicking on create, repeat the creation steps again.

    > Once the ECS cluster creation is completed, create Task definition which wll have the informtion about the container like from where the images are pulled, cpu, RAM, etc.

    - click on `Task definitions` --> click on 'create new task definition'
        - 