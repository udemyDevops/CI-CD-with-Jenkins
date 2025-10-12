* Jenkins pipeline can be executed automatically using triggers instead of clicking the 'Build now' button manually.

## Popular triggers
* Git Webhook --> Git repo will trigger the jenkins job whenever a commit or any changes made to the repo (Git repo will send a JSON payload)
* Poll SCM --> opposite of Git Webhook. Here Jenkins will check for commits in the git repo with a frequency that we set and trigger the job when a commit is found
* Scheduled jobs --> like cron jobs, jenkins will make sure to run the jobs as per the defined schedule
* Remote triggers (complex) --> trigger the job from anywhere like using a script, ansible playbook, etc which uses tokens, secrets, URLs, etc. Here we get an api call which can be used to trigger the job
* Build after other projects are built --> trigger is completoin of a previos job, we need to configure the sequence of jobs to trigger the subsequent job

## Steps
* [Create a Github repository](#git-repo)
* [SSH authentication of git repo](#ssh-authentication-of-git-repo)
* [Create a Jenkinsfile in get repo and commit](#commiting-a-jenkinsfile-to-repo)
* [Create a jenkins job to access Jenkinsfile from git repo](#creating-a-job-in-jenkins-and-adding-credentials-to-use-the-ssh-key)
* [Test the triggers](#testing-the-triggers)

### Git Repo
* create a Github repository
    - Gihub repo --> we'll use the ssh link of git repo
        > git@github.com:[organization name]/[repo name].git

    - Before using this link, we need to store the ssh keys in Github account


### SSH authentication of Git repo
* Set ssh suthentication to Github repo
    - Create ssh keys
    - open command prompt
        ```
        ssh-keygen -t rsa -b <size 2048 or 4096> -f <path and file name>
        ```
    > **OR**

    - open GitBash and run the same command as above or can use the below command
        ```
        ssh-keygen.exe
        ```
    - Copy the public key content
    - Go to Gihub account --> settings (account settings) --> SSH and GPG keys --> click on `New SSH key` --> give a title and paste the public key content --> click on `Add SSH key` to save it.

### Commiting a Jenkinsfile to repo
* Create a Jenkinsfile in get repo and commit
    - clone the repo
        ```
        git clone git@github.com:<organization name>/<repo name>.git
        ```
    - create a `Jenkinsfile` [_samplefile_](Jenkinsfile)
    - add and commit the file to repo (main branch - need to change the default bracnh to main from master)
        ```
        git add. ; git commit -m "build trigger test"; git push origin main
        ```
        > **OR** of using master brach use the below command
        ```
        git add. ; git commit -m "build trigger test"; git push origin master
        ```

### Creating a job in jenkins and adding credentials to use the ssh key
* Create a jenkins job to access Jenkinsfile from git repo
    - Jenkins tries to ssh git repo using the key and encounter 'Host key verfification failed'. This is becuase whenever we login to linux using ssh for the first time there would be a question (Fingerprint, do you accept the connection) to enter yes/no. Jenkins will also face this question if the host key checking is enabled
    - To avoid this --> Jenkins dashboard --> manage jenkins --> security --> Configure Global security --> scroll down to find `Git Host key verification configuration` --> select `Accept first connection` for 'Host key verification strategy' and click on `Apply` and then `save`
    - Now create a job in Jenkins --> `+ New Item` --> Give a name and select item type as `pipeline` and click ok
        - Under Pipeline --> Definition --> Pipeline script from SCM
            - SCM --> Git
                - Repository URL (SSH URL)
                - Credentials
                    - Now click on `Add` --> select Jenkins (credentials provider)
                        * Under `Kind` select `SSH Username with private key`
                        * For Id, give a name to identify it as SonarQube token
                        * add a description
                        * Username --> is the github account (organization name)
                        * private key --> enter directly --> Add --> paste the private key content (including the begin and end statements of the key)
                        * Click on Add to save the credential
                    - From the drop down select the saved credential
                - Branch --> */main or (*/master if using master branch), as we are storing the file in the main/master branch for this exercise
            - Script Path --> path of the `Jenkinsfile` (if it is in the root directory of the branch just mentoin the file name or if it is located in a folder eg: /trigger/Jenkinsfile)
        - save --> build now

### Testing the triggers

#### Git WebHook
* To trigger the pipeline based on a commit (or other events in git repo)
* Copy the Jenkins URL --> http://[jenkins server public IP or DNS record]:8080/ (can also use https)
    > eg: http://18.208.57.13:8080/
* In the Github repo --> settings --> code and automation --> webhooks --> click on `Add webhook`
    - Payload URL --> paste the jenkins URL and add `github-webhook/`
        > eg: http://18.208.57.13:8080/github-webhook/
    - Content type --> select 'application/json'
    - Select the events to trigger the webhook --> select **push** event 
        > apart from push we can also select **everything** or **individual events**
    - click on `Add webhook` to save it.
    > once it is added we should see a green tick mark for the webhook indicating the last delivery as successfull, if not refresh the page. Click on the webhook --> REcent deliveries. If we see a failure (red mark), check if the Jenkins URL and content type are correct. Also, check the Jenkins server SG has an inbound rule allowing port 8080 from anywhere (0.0.0.0/0)
* Go to the jenkins job created in previous section --> configure --> Build triggers --> select `GitHub hook trigger for GITScm polling` --> Save
* Now to test the trigger by adding a sample text file and commit to repo which should trigger the job
> We can see 'GitHub Hook Log' added to the pipeline dashboard

#### Poll SCM
* Go to the jenkins job created in previous section --> configure --> Build triggers --> select `Poll SCM` --> Schedule --> add a schedule in cronjob format (Minute Hour DOM Month DOW) --> save
    > eg: * * * * * --> every minute

    > Minute --> 0 to 59; Hour --> 0 to 23 DOM --> day of month (1 to 31); Month --> 0 to 12; DOW --> Day of week (0 to 7 where o & 7 are Sunday)

* Now to test the trigger by adding a sample text file and commit to repo which should trigger the job
> We can see 'Git Polling Log' added to the pipeline dashboard 

#### Scheduled Jobs
* Here jenkins will run the pipeline as per the schedule and not based on the commits as it will not check for changes in repo
* Go to the jenkins job created in previous section --> configure --> Build triggers --> select `Build periodically` --> Schedule --> add a schedule in cronjob format (Minute Hour DOM Month DOW) --> save
    > eg: 30 20 * * 1-5 --> run at 8:30 PM every Monday to Friday

#### Remote Trigger
Refer the below documents for steps to setup remote trigger
- [_Build+Triggers+Remotely.docx_](Build+Triggers+Remotely.docx)
- [_Build+Triggers+Remotely.pdf_](Build+Triggers+Remotely.pdf)

> Remote trigger - can trigger the Jenkins job from anywhere, from a script, from other Jenkins server, from our laptop, from some other server, anywhere we can run that job, as long as we have the network access to the Jenkins server.

* _Generate JOB URL_
    * Go to the jenkins job created in previous section --> configure --> Build triggers --> select `Trigger builds remotely` --> Authentication token --> can give a name for token (eg: mybuildtoken), the same name should be there in the token URL (eg: JENKINS_URL/job/JOB_NAME/build?token=TOKEN_NAME) --> copy the URL --> click on `save`
    ![_Job URL_](jobUrl-buildToken.png)
    * Update the URL
        - JENKINS_URL --> http://JenkinsServerIP:8080
        - JOB_NAME --> name of the pipeline job (eg: Build)
        - TOKEN_NAME --> name of the token (eg: mybuildtoken)
    * Now the JOB URL will be like eg: 'http://JenkinsServerIP:8080/job/Build/build?token=mybuildtoken'

* _Generate Token for user_
    * In the Jenkins, go to the user (logged in with) on top right corner --> configure --> API Token --> Add new token --> `Generate` --> Copy the token (it should be appended to the username) to be used in 'curl' command (username:APIToken) later
        > username:token --> eg: admin:86df83e602414bae9f0ad90588a92233


* _Generate CRUMB_
    * Run the below command (update the values)
    ```
    wget -q --auth-no-challenge --user username --password password --output-document - 'http://JekninsserverIP:8080/crumbIssuer/api/xml?path=concat(//crumbRequestField,":",//crumb)'
    ```
    > In Windows, wget command is not available by default and need to be installed. For this exercise we use wget for Git Bash (https://gist.github.com/evanwill/0207876c3243bbb6863e65ec5dc3f058 --> https://gist.github.com/evanwill/0207876c3243bbb6863e65ec5dc3f058#wget) --> download 64-bit zip file --> open the zip file --> select(single click) `wget.exe` and and click `Extract to` the path "C:\Program Files\Git\mingw64\bin" and click 'ok'

    > Now, open Git Bash and run the command above --> it will give the crumb (eg: Jenkins-Crumb:86df83e602414bae9f0ad90588a9223306ab5e103d7749a88f5bc92ff505c183) --> copy it to be used in 'curl' command

* After generating the JOB URL, token and CRUMB --> Run the below 'curl' command (update the values) from antwhere (eg: Git Bash)
```
curl -I -X POST http://username:APItoken@Jenkins_IP:8080/job/JOB_NAME/build?token=TOKENNAME
-H "Jenkins-Crumb:CRUMB"
```
> Syntax of the command --> curl -I -X POST http://(TokenForUser)@(JobURL without http://) -H "CRUMB"

> eg: curl -I -X POST http://admin:86df83e602414bae9f0ad90588a92233@JenkinsServerIP:8080/job/Build/build?token=mybuildtoken -H "Jenkins-Crumb:86df83e602414bae9f0ad90588a9223306ab5e103d7749a88f5bc92ff505c183"
