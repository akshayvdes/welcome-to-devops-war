pipeline {
	agent none
environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub_slave-1')
	}
    stages {
	
       stage('checkout') {
    agent  { label 'tom' }
            steps {
                sh 'sudo rm -rf welcome-to-devops-war'
	sh 'git clone https://github.com/akshayvdes/welcome-to-devops-war.git'	
              }
        }
	 stage('build') {
 agent  { label 'tom' }
	steps {
 
                dir('welcome-to-devops-war'){
                  sh 'pwd'
                sh 'ls'
            
                sh 'docker build -t tomcat:$BUILD_NUMBER .'  
                }
            }
	 }
	 stage('deploy'){
 agent  { label 'tom' }
	     steps{
	        sh 'docker rm -f mytomcat'
	         sh 'docker run -d --name mytomcat -p 8888:8080 tomcat:$BUILD_NUMBER'
	     }
	 }
		stage('Login') {
agent  { label 'tom' }
			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}
	stage('Push') {
 agent  { label 'tom' }

			steps {
			    sh 'docker tag tomcat:$BUILD_NUMBER akshayvdes/tomcatnew_ak:$BUILD_NUMBER'
				sh 'docker push akshayvdes/tomcatnew_ak:$BUILD_NUMBER'
			}
		}

    stage('pull image'){
    agent { label 'deplo' }
        steps{
            sh 'docker rm -f mytomcat'
            sh 'docker run -d --name mytomcat -p 7100:8080 akshayvdes/tomcatnew_ak:$BUILD_NUMBER'
        }
    }
    }
}
            
            
