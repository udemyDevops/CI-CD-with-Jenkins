
## Docker CI flow

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
    > To run the docker commands we need the docker engine