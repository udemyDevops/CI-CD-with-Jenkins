# Pipeline as a code

* To automate the pipeline setupe with a file called --> 'Jenkinsfile'
    ![_paac-intro_](paac-intro.png)

[_sample-file_](Jenkinsfile)
> doc: https://www.jenkins.io/doc/book/pipeline/

* In Jenkins --> '+ New Item' --> Give a name and select item type as 'pipeline' and click ok
    - has 3 sections
        1. General
        2. Advanced Project options
        3. Pipeline
            - Under Pipeline --> Definition --> 2 options
                1. pipeline script (paste the pipeline script)
                2. pipeline script from scm (fetch the file from a given path in repo)
            - Save --> Build now

* Once the pipeline job is completed, can view the output --> build history --> click on the success/failed button beside the job number (eg: #1).

* To view the individual stage --> click on 'stages' --> click on any stage to see the respective stage status/output