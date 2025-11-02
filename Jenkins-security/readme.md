* This section covers the Authentication and Authorization in Jenkins. In the previous sections all configurations and jobs are done using Admin user.
* In Jenkins, once we create a pipeline it may be handed-over to the respective team (developers, testers, OPs or non-OPs). The teams cannot use Admin user, should use their IDs with the permissions set as per the requirement

![alt text](Securing-Jenkins.png)


* Manage Jenkins --> security --> configure Global security --> 

    - **Authentication** --> security realm

        ![alt text](security-realm.png)

        1. Jenkin's own user database --> Allow users to sing up (Users with the Jenkins URL can sign up themself)
            - Once the user sign up, the Admin user have to provide permissions --> Manage Jenkins --> security --> configure Global security --> Authorization --> click `add user or group` --> give the user name (if the user is not part of Jenkin's database a cross 'x' mark wuill be shown for the user logo)
        2. LDAP --> to integrate users in AD


    - **Authorization**
        * Matrix-based security - permissions at Jenkins level
            - click `add user or group` as admin user --> 

            ![alt text](addUser.png)
            ![alt text](userPermissions.png) 

        * Project-based matrix authorization strategy - permissions at project / job level
            - click `add user or group` as admin user --> 

            ![alt text](addUser.png)
            ![alt text](userPermissions.png)

            - As admin user, go to a job (where the user need access) --> configure --> 
                * Under General --> tick the box for `enable project-based security` --> click `add user or group`

                ![alt text](job-specific-permissions.png)


* In case of many users, we can create a `role` and assign it to users. For this we need a plugin
    - Easy to manage permissions as the roles can be defined early and the user can be added as and when required.
    - manage jenkins --> manage plugins --> Available --> search for `Role-based Authorization strategy`and install it
    - once the plugin is installed --> Manage Jenkins --> security --> configure Global security --> Autorization
        * select `Role-based strategy`

        ![alt text](role-based-strategy.png)

    - Now a new option `manage and assign roles` will be visible under Manage Jenkins --> security
        * Manage Jenkins --> security --> manage and assign roles
            - manage roles --> click `add` to add a role and provide permissions. This role can be assigned to the users as per the requirement

            ![alt text](addRole.png)

            - assign roles --> click `add` to add a user and assign the role

            ![alt text](assignRole.png)


* We can also add users in Jenkins which will be added to Jenkins database (if Jenkin's own user database is selected for Authentication)
    - Manage Jenkins --> security --> manage users
