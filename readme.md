* Jenkins is extensible (it's features can be extended using plugins)

* This covers the below sections
    - [_Basics in Jenkins_](Basics)
    - [_CI piepline flow and pipeline code_](CI-pipeline)
        - The above sections cover complete CI pipeline but the 
            - Start (not automatic but manual trigger) --> if a commit is made to the repo it should trigger the job automatically
            - End --> to get notifications about the pipeline job, whether passed/failed. Ther are multiple plugins available in jenkins, using them we can send notifications to different communication platforms/tools like slack, microsoft teams, etc. Refer [_md file to setup notification_](CI-pipeline/pipeline-notification.md)
    - [_Docker CICD_](Docker-CI-CD)
        - In Prevoius sections the artifacts are published in CI pipeline. Here we build the docker images, which contains artifacts, publish to container registry using CI pipeline and deploy the image
    - [_Build Triggers_](Build-triggers)
        - In previous sections the pipeline jobs were triggered manually by clicking on 'Build now' button in jenkins. Here we see how to run the job automatically when a commit is made to the repo
    - [_Agent/Node/Slave concept_](Agent-Node-Slave)


### Jobs in Jenkins
* Freestyle
* Pipeline as a code

