## Flow of Continuous integration pipeline

![CI-pipeline-flow](CI-pipeline-flow.png)

## Steps to setup CI pipeline
1. Jenkins server
2. Nexus server
3. Sonarqube server
4. Security group
5. [_Plugins_](#plugins)
6. Integrate Nexus and Sonarqub with Jenkins
7. Write pipeline script
8. Set notification (to get alert when a job is failed)

## Setup Jenkins, Nexus, Sonarqube and Security group

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

> Nexus home directory --> '/opt/nexus/'

    1. Check the nexus service status --> 'systemctl status nexus'
    2. Check java version --> 'java -version'
    3. Initial password of nexus --> '/opt/nexus/sonatype-work/nexus3/admin,password' (path --> can get when we sign-in in the browser --> <server IP>:8081)


* **Sonar** --> [_sonar-script_](userData/sonar-setup.sh)  (will setup 3 services --> sonarqube, postgresql and nginx)
    - Ubuntu 24 (t2.medium)
    - Security Group rules
        * Allow ssh port 22 from client
        * Allow port 80 from client to access nginx (frontend for sonarqube)
        * Allow port 80 from Jenkins SG (Jenkins to upload code analysis results to sonarqube server)

> sonarqube service takes more resource, so need to make some OS level changes at path '/etc/sysctl.conf' and '/etc/security/limits.conf'. Changes to these files needs the server reboot (reboot command included at the end of script)

> Access sonarqube in the browser --> <server IP>. username and password is 'admin' --> change it after login 


* **Jenkins** --> [_jenkins-script_](userData/jenkins-setup.sh)
    - Ubuntu
    - Security Group rules
        * Allow ssh port 22 from client
        * Allow port 8080 from client to access Jenkins site
        * Allow port 8080 from sonarqube SG (sonarqube send the test results, status to jenkins)

> Usernames for 
 
    - Amazon linux -- ec2-user
    - ubuntu -- ubuntu


## Plugins
    - Nexus
    - Sonarqube
    - Git (installed by default in jenkins)
    - Pipeline Maven integration (to write pipeline as a code)
    - pipeline utility steps
    - Build timestamp

* Manage Jenkins -->  manage plugins --> under plugin manager, select Available and search for the above plugin
> Nexus artifact uploader, SonarQube Scanner 