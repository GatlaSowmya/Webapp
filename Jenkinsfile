pipeline {
    agent any
   tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    } 
   
    stages {
        stage('GitCodeCheckOut') {
            steps {
                git branch: 'main', url: 'https://github.com/GatlaSowmya/Webapp.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('ImageBuild') {
            steps {
                sh 'docker build -t sowmya018/jenkins_task:$BUILD_ID .'
            }
        }
        stage('Dockerhublogin') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', passwordVariable: 'PASS', usernameVariable: 'USER')])  {
              sh 'docker login'
        }
            }
        }
        stage('Dockerhubpush') {
            steps {
                sh 'docker push sowmya018/jenkins_task:$BUILD_ID'
            }
        }
        
    }
}
