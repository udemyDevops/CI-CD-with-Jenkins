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

> Bash scripts 

    https://github.com/hkhcoder/vprofile-project --> branch --> atom --> userdata
    
    or

    https://github.com/udemyDevops/CI-CD-with-Jenkins --> CI-pipeline --> userData

* **Nexus** --> [_nexus-script_](userData/nexus-setup.sh)
    - Amazon linux 2023 (t2.medium)
    - Security Group rules
        * Allow ssh port 22 from client
        * Allow port 8081 from client to access Nexus site
        * Allow port 8081 from Jenkins SG (to allow jenkins server to communicate with Nexus, to push the artifact to Nexus)

* **Sonar** --> [_sonar-script_](userData/sonar-setup.sh)  (will setup 3 services --> sonarqube, postgresql and nginx)
    - Ubuntu 24 (t2.medium)
    - Security Group rules
        * Allow ssh port 22 from client
        * Allow port 80 from client to access nginx (frontend for sonarqube)
        * Allow port 80 from Jenkins SG (Jenkins to upload code analysis results to sonarqube server)

> sonarqube service takes more resource, so need to make some OS level changes at path '/etc/sysctl.conf' and '/etc/security/limits.conf'. Changes to these files needs the server reboot (reboot command included at the end of script)


* **Jenkins** --> Ubuntu

> Usernames for 
 
    - Amazon linux -- ec2-user
    - ubuntu -- ubuntu
