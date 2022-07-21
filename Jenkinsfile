pipeline {
    agent any
    tools {
        maven 'Maven3.8.6'
    }
    stages {
        stage('Hello') {
            steps {
                 checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/keyspaceits/javawebapp.git']]])
            }
        }    
        stage('build'){
            steps{
                sh 'mvn clean install -f pom.xml'
            }   
        }    
        stage('deploy'){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat-deployer', path: '', url: 'http://65.0.190.32:8080')], contextPath: null, war: '**/*.war' 
            }
        } 
    }
}
