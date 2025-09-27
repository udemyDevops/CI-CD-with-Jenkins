1. [_Tools_](#tools)
2. [_Plugins_](#plugins)

## Tools
> Jenkins need the tools to be installed on the OS and also the plugin.

    Jenkins by default comes with Maven plugin but the tool needs to be installed/added.
    Manage Jenkins --> tools --> Maven Installations -- Add Maven

> For vprofile we need

    - git --> to clone source code
    - maven --> to build the code which inturn need
        * jdk/java

* all this need to be done 
    - using plugins (Manage jenkins --> Plugins --> install the selected plugin)
    - install the tools on the OS where jenkins is running
> tools can be installed either running the commands by doing ssh or use the jenkins UI (Manage jenkins --> tools --> Add (for selected tool), if the tool is not found then install the plugin)

> some tools may not show the versions in jenkins UI which need to be installed manually by running commands in the server

> eg: java/jdk

> Install the jdk-17, assuming jdk-21 is already installed

> 'apt install openjdk-17-jdk' --> 'java -version' can see the 21 as the default version --> 'ls /usr/lib.jvm/' java home directory, can see the installed jdk versions

> so the JAVA_HOME required for jdk tool in Jenkins UI is '/usr/lib.jvm/java-(version)-openjdk-amd64'


## Plugins
> 

    Updates
    Availbale plugins
    Installed plugins
    Advanced settings

* plugin for time stamp --> Build Timestamp, which we can use to store the artifact with time stamp
> this plugin will add a variable BUILD_TIMESTAMP

> After installing this plugin, we need to set the format, timezone...etc

    System --> Build Timestamp (willsee only if the plugin is installed)

* Plugin for s3 bucket --> to store the artifacts instead of jenkins workspace (which is not permanent)
