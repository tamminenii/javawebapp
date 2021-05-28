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
                withSonarQubeEnv('SonarQubeServer') {
                sh 'mvn sonar:sonar -f pom.xml'
                }
            }
        }
        stage('Dev Deploy'){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat-user', path: '', url: 'http://3.219.228.196:8080/')], contextPath: 'CounterWebApp', war: '**/*.war'
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
                deploy adapters: [tomcat9(credentialsId: 'tomcat-user', path: '', url: 'http://3.219.228.196:8080/')], contextPath: 'CounterWebApp', war: '**/*.war'
            }
        }
    }
}
