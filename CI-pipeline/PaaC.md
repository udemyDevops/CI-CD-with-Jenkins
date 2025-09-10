# Pipeline as a code

* To automate the pipeline setupe with a file called --> 'Jenkinsfile'
    ![_paac-intro_](paac-intro.png)

[_sample-file_](PipelineCodeDemo.txt)

    pipeline {
        agent any
        tools {
            maven "MAVEN3.9"
            jdk "JDK17"
        }

        stages {
            stage('Fetch code') {
                steps {
                git branch: 'atom', url: 'https://github.com/hkhcoder/vprofile-project.git'
                }

            }


        stage('UNIT TEST') {
                steps{
                    sh 'mvn test'
                }
            }


            stage('Build'){
                steps{
                sh 'mvn install -DskipTests'
                }

                post {
                success {
                    echo 'Now Archiving it...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
                }
            }
            
        }
    }