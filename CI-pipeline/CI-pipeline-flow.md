## Flow of Continuous integration pipeline

![CI-flow](CI-pipeline-flow.png)

## Steps to setup CI pipeline
1. Jenkins server
2. Nexus server
3. Sonarqube server
4. Security group
5. Plugins
6. Integrate Nexus and Sonarqub with Jenkins
7. Write pipeline script
8. Set notification (to get alert when a job is failed)

## Setup Jenkins, Nexus and Sonarqube
* Nexus --> Amazon linux 2023
* Sonar --> Ubuntu 2024
* Jenkins --> Ubuntu
> Bash scripts 

    https://github.com/hkhcoder/vprofile-project --> branch --> atom --> userdata
    
    or

    https://github.com/udemyDevops/CI-CD-with-Jenkins --> CI-pipeline --> userData

