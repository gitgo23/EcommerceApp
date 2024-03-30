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
                dir('EcommerceApp') {
                    sh '''
                    mvn clean
                    mvn test
                    mvn package
                    '''
                } 
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
                repository: 'EcommerceApp-release', 
                version: '0.0.1-RELEASE'
            }
        }

        stage('Deploy to UAT') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'TOMCAT', path: '', url: 'http://3.93.69.93:8080/')], 
                contextPath: null, 
                war: 'target/*.war'
            }
        }

    }
}