## First job
> A job in jenkins is a set of instructions/tasks we want to execute. We can select the job execution to be on jenkins server or a node.

* To create a job, Dashboard --> click on '+ New Item' or 'create a job'
> '+ New Item' --> give a name and select item type as 'freestyle project' (for practice) --> ok

> The configuration has certain sections
    
    1. General
    2. Source Code Management
    3. Build Triggers
    4. Build Environment
    5. Build Steps
    6. Post-build Actions

> All these are covered later, here we focus on 'Build Steps' which has certain instructions based on the installed plugins

> eg: select 'Execute Shell', add some commands and save
    
    whoami
    pwd
    w
    id

* In the Job dashboard --> Click on 'Build Now' --> Under 'Build history' we can see the job status

> Workspace in job dashboard --> it is not a permanent store for data, meant only for the data generated when the job executes. Need to move this data to some other storage if required.

    eg: cat /proc/cpuinfo > cpuinfo.txt
    
    After the gets completed the 'cpuinfo.txt' file will be added in the workspace

