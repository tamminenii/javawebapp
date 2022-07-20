pipeline {
    agent any
    tools {
        maven 'Maven3.8.6'
    }
    stages {
        stage('checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/keyspaceits/javawebapp.git']]])
            }
        }
        stage('build'){
            steps{
                sh 'mvn clean install -f pom.xml'
            }
        }
        stage('CodeQulity'){
            steps {
            withSonarQubeEnv('SonarQube'){
            sh 'mvn clean install -f pom.xml sonar:sonar' 
              }
            }
        }
        stage('deploy'){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat-deployer', path: '', url: 'http://192.168.1.200:8080')], contextPath: null, war: '**/*.war'
            }
        }
        
    }
}
