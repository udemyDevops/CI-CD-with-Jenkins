* Jenkins pipeline can be executed automatically using triggers instead of clicking the 'Build now' button manually.
### Popular triggers
    - Git Webhook --> Git repo will trigger the jenkins job whenever a commit is made to the repo (Git repo will send a JSON payload)
    - Pool SCM --> opposite of Git Webhook. Here Jenkins will check for commits in the git repo with a frequency that we set and trigger the job when a commit is found
    - Scheduled jobs --> like cron jobs, jenkins will make sure to run the jobs as per the defined schedule
    - Remote triggers (complex) --> trigger the job from anywhere like using a script, ansible playbook, etc which uses tokens, secrets, URLs, etc. Here we get an api call which can be used to trigger the job
    - Build after other projects are built --> trigger is completoin of a previos job, we need to configure the sequence of jobs to trigger the subsequent job

### Steps
* create a Github repository
    - Gihub repo --> we'll use the ssh link of git repo
        > git@github.com:[organization name]/[repo name].git
    - Before using this link, we need to store the ssh keys in Github account
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
* Create a Jenkinsfile in get repo and commit
    - clone the repo
        ```
        git clone git@github.com:<organization name>/<repo name>.git
        ```
    - create a `Jenkinsfile` [_samplefile_](Jenkinsfile) sand
* Create a jenkins job to access Jenkinsfile from git repo
* Test triggers




