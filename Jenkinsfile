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
                ScannerHome = tool 'sonar5.0'
            }
            steps {
                script {
                    withSonarQubeEnv('sonarqube') {
                        sh "${ScannerHome}/bin/sonar-scanner -Dsonar.projectKey=EcommerceApp"
                    }
                }
            }
        }

    }
}