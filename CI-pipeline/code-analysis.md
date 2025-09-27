In [_PaaC-intro md file_](PaaC-intro.md) the stages of fetching code, testing and build is covered. The next step is code analysis (![CI-pipeline-flow](CI-pipeline-flow.png)) which is to check below aspects to improve the code quality
- the code against the best practices
- vulnerabilites (like top 10 OWASP - Open Worldwide Application Security Project)
- functional errors (bugs) before deployment
- etc..

* Some tools for code analysis -- Checkstyle, Cobertura, mstest, owasp, SonarQube Scanner, etc. For this practice we'd be using Checkstyle and SonarQube

* To write pipeline as code and integrate code analysis in it we need to first integrate the SonarQube server with jenkins. Also, we need to install SonarQube scanner tool in jenkins

### Integrating Sonar Qube with Jenkins
* Make sure the Jenkins and SonarQube servers are up and running
* Login to Jenkins and SonarQube (in browser -- [_CI-pipeline-flow md file_](CI-pipeline-flow.md)) 