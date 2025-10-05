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
* Go to the jenkins job created in previous section --> configure --> Build triggers --> select `Poll SCM` (unselect the github webhook trigger 'GitHub hook trigger for GITScm polling') --> Schedule --> add a schedule in cronjob format (Minute Hour DOM Month DOW) --> save
    > eg: * * * * * --> every minute

    > Minute --> 0 to 59; Hour --> 0 to 23 DOM --> day of month (1 to 31); Month --> 0 to 12; DOW --> Day of week (0 to 7 where o & 7 are Sunday)

* Now to test the trigger by adding a sample text file and commit to repo which should trigger the job
> We can see 'Git Polling Log' added to the pipeline dashboard 
