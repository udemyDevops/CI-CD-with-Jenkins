This section covers the Jenkins Master-Slave concept. In the previous sections the jobs are run on Jenkins master server. But in some scenario or use cases where we need to run jobs from some other machines which will be the slave machines for Jenkins.

### Common use-cases
* load distribution or distributed builds
    > when using Jenkins at an organization level and there are many jobs getting triggered automatically, people are executing jobs which may not be possible for Jenkins to run all those jobs. So, if we add a node as a slave to Jenkins, then Jenkins can decide if the job has to be running on master or on the slave. Jenkins will pick up an available slave and execute the job on the slave. Whether we're executing or cloning the source code, we're running Maven commands, we're running software tests, these things will be running on the slave.

* cross-platform builds
    > If we're running Jenkins on a Linux machine and need to build a Windows-based package, we need some Windows tools like MS Build which we cannot execute that on Linux machines. So, we will add a Windows machine as a slave to Jenkins and we can run the specific job, say, "Run my Windows job only on this Windows machine." Or same for MacOS.

* Software testing
    > When doing continuous delivery and need to include the software testing. Software testers or the QA team will be writing software test cases. They will be mostly executing it from a Windows machine, so it'll open a browser graphically and execute the test cases. Software testers will have some machine which can be added as a slave, and we can run the software test cases from Jenkins. So we can include that job also in our pipeline. And for this, you really need to work very closely with the software testers.

* To run the scripts like shell script, Python script, Ansible playbook we can have a separate machine that runs the script by adding it as a slave to run any command or a script. It can be Windows machine or Linux machine or MacOS or someone's laptop.