pipeline {
    
    agent any
    tools{
        maven 'Maven3'
    }
    stages{
        stage('checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/keyspaceits/javawebapp']]])
            }
        }
        stage('build'){
            steps {
                sh 'mvn clean install -f pom.xml'
            }
        }
        stage('Code Quality') {
            steps {
                withSonarQubeEnv('SonarQube') {
                sh 'mvn sonar:sonar -f pom.xml'
                }
            }
        }
        stage('Dev Deploy'){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat9', path: '', url: 'http://3.215.85.234:8080/')], contextPath: 'CounterWebApp', war: '**/*.war'
            }
        }
        stage('Dev apprl for QA') {
            steps {
                echo "taking approval from Dev Manager"
                timeout(time: 7, unit: 'DAYS') {
                    input message: 'Do you want to proceed to QA?', submitter:'admin'
                }
            }
        }
        stage('QA Deploy'){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat9', path: '', url: 'http://3.215.85.234:8080/')], contextPath: 'CounterWebApp', war: '**/*.war'
            }
        }
    }
}
