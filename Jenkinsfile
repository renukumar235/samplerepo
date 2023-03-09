pipeline {
    agent any

    tools {
        // Install the Maven version configured as "Maven3.9" and add it to the path.
        maven "maven3.9"
    }

    stages {
        stage('checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/renukumar235/java-app.git']])
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean install -f pom.xml'
            }
        
        }
        stage('build-notify') {
            steps {
                slackSend channel: 'channel01', message: 'build success', tokenCredentialId: 'slack'
            }   
        }
        stage('Deploy to QA') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://107.22.246.158:8080')], contextPath: null, war: '**/*.war'
            }
        }
        stage('Deploy-notify') {
            steps {
                slackSend channel: 'channel01', message: 'Deploy success', tokenCredentialId: 'slack'
            }
        }
    }
}
