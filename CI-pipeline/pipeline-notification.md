* Jenkins Dashboard --> manage jenkins --> plugins --> available plugins --> search for 'notification' --> select `Slack Notification` for this practice --> click on `Install`
> can create a free tier slack account
* Add the slack account details in Jenkins
    - To integrate slack with jenkins we need a token which we can get by installing jenkins app in slack
    - login to slack account
    - search for slack apps in google --> slack marketplace --> search for jenkins --> select `jenkins CI` --> click on `Add to Slack` --> select the channel and click on `Add Jenkins CI integration`
    - We can see the token under 'step-3' or scroll down till Integration settings to see or regenrate token
    - Copy the token and click on `save settings`
    - Now copy the slack workspace name from url (click on the workspace --> can see worksapce name and url)
        > For example, if the workspace name is 'sprojex' then the url look like 'https://sprojexworkspace.slack.com'. For jenkins we need to give 'sprojexworkspace'
    - In Jenkins --> manage jenkins --> system --> search (ctr+F) for 'slack'
        - workspace -- give the workspace got from url (eg: sprojexworkspace)
        - Under Token, click on `Add` --> select Jenkins (credentials provider)
            * Under `Kind` select `Secret text`
            * Paste the slack token in `Secret`
            * For Id, give a name to identify it as slack token
            * add a description
            * Click on Add to save the token
        - From the drop down select the saved token
        - Default channel / memberID --> provide the channel name (eg: #devops)
        - Click on `test connection` to verify and if it is success click on `save`

* pipeline code with notification [_Jenkinsfile with notification_](Jenkinsfile-with-Notification). Change the file name to Jenkinsfile and update the values in stages before using this.

    - In Jenkins --> `+ New Item` --> Give a name and select item type as `pipeline` and click ok
        - Under Pipeline --> Definition --> Pipeline script --> paste the pipeline script
        - save --> build now