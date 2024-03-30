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
                sh "mvn clean"
            }
        }

        stage('Test with Maven') {
            steps {
                sh "mvn test"
            }
        }

        stage('Package with Maven') {
            steps {
                sh "mvn package"
            }
        }
    }
}