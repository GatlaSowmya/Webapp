pipeline {
    agent any
    environment {
        USER = 'sowmya018'
        PASS = 'docker@018'
    }
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
         stage('test') {
            steps {
                sh 'mvn sonar:sonar'
            }
        }
        stage('ImageBuild') {
            steps {
                sh 'docker build -t sowmya018/jenkins_task:$BUILD_ID .'
            }
        }
        stage('Dockerhublogin') {
            steps {
               
              sh 'docker login -u $USER -p $PASS'
        
            }
        }
        stage('Dockerhubpush') {
            steps {
                sh 'docker push sowmya018/jenkins_task:$BUILD_ID'
            }
        }

        stage('deploy'){
            steps{
                 sshagent(['jenkins']) {
                            sh '''
                                ssh ubuntu@15.188.246.110 'docker pull sowmya018/jenkins_task:$BUILD_ID'
                                ssh ubuntu@15.188.246.110 'docker run -d --name sample -p 8080:8080 sowmya018/jenkins_task:$BUILD_ID'
                            '''
                        }
            }
    }
}
