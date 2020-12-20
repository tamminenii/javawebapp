pipeline {
    agent any
    tools {
        maven 'Maven3' 
    }
    stages{
        stage('checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/keyspaceits/javawebapp']]])
            }

        }
        stage('build') {
            steps{
                sh 'mvn clean install -f pom.xml'
            } 
        }
        stage('code quality'){
            steps{
                withSonarQubeEnv('SonarQubeServer') {
                    sh 'mvn sonar:sonar -f pom.xml'
                }
            } 
        }
        stage ('DEV Deploy') {
            steps {
                echo "deploying to DEV Env "
                deploy adapters: [tomcat9(credentialsId: 'tomcat-user', path: '', url: 'http://3.219.228.196:8080/')], contextPath: 'CounterWebApp', war: '**/*.war'
            }
        }
        stage ('slack notify') {
            steps {
                slackSend channel: 'keyspace-team', message: 'Dev Deployment was successful'
            }
        }
   }
}
