pipeline {
    agent { label 'jenkins-agent' }
    tools {
        jdk 'jdk17'
        maven 'Maven3'
    }
   
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/devops-aws-17/youtube-registerapp.git'
                }
        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }
        stage("SonarQube Analysis"){
           steps {
	           script {
		           withSonarQubeEnv(credentialsId: 'sonar') { 
                   sh "mvn sonar:sonar"
		        }
	           }	
           }
       }
       stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar'
                }	
            }

        }
    }
}
       
