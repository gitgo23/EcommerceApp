pipeline {
    agent any
    tools {
        maven "Maven"
    }

    stages {

        stage('Git Clone') {
            steps {
                git 'https://github.com/gitgo23/EcommerceApp.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh '''
                mvn clean
                mvn test
                mvn package
                '''
            }
        }

        stage('SonarQube Analysis') {
            environment {
                ScannerHome = tool 'Scanner-5'
            }
            steps {
                script {
                    withSonarQubeEnv('sonarqube') {
                        sh "${ScannerHome}/bin/sonar-scanner -Dsonar.projectKey=EcommerceApp -Dsonar.java.binaries=target/EcommerceApp/WEB-INF/classes"
                    }
                }
            }
        }

        stage('Upload to Nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'EcommerceApp', 
                classifier: '', 
                file: 'target/EcommerceApp.war', 
                type: 'war']], 
                credentialsId: 'NEXUS_CRED', 
                groupId: 'com', 
                nexusUrl: '34.229.186.51:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'EcommerceApp-Snapshot', 
                version: '0.0.1-SNAPSHOT'
            }
        }

    }
}