## First Build job using Maven
* Add the jdk-21 in jenkins tools (same as for jdk-17 done in [_manageJenkins_](manageJenkins.md))
> make sure to have the jdk-21 installed before adding the tool

* '+ New Itme' --> give a name and select item type as 'freestyle project' --> ok
* Under General 
    - provide description
    - select the JDK version (from the list as per the versions added under tools and requirement)

* Source Code Management --> Select 'Git'
    - provide the 'repository url' of source code
    > eg: https://github.com/hkhcoder/vprofile-project.git
        
    - Credentials --> For a public repository we don't need credentials but required for private repository
    > Click on 'Add' to add credentials.
    - Branches to build --> Branch specifier (*/atom -- branch name for vprofile)

* Build Steps
    - select 'Invoke top level maven targets' (if the plugin is already installed)
    - For maven version --> select the tool added for maven (or select 'Default' if the maven is installed in the jenkins server, if not selecting 'Default' will fail the job with error no 'mvn' command found)
    - Goals --> 'install' (same as build command --> mvn install)
    > Under Advanced --> we can specify other details, like the path for 'pom.xml' if in a different folder

* Post-Build Actions
    - Select 'Archive the artifacts' --> under 'Files to archive' --> '**/*.war' (to search for .war files recursively, all sub folders, in the workspace)
    > This will keep the artifact in a different location so that even if the workspace is wiped out we can retreive the artifact from there by going to job status

### Practice
* create a test job and use jdk11 --> for this use branch 'jdk11' in 'https://github.com/hkhcoder/vprofile-project.git
* Install jdk-11 in jenkins server and add the tool by giving the path