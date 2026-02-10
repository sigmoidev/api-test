pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                script {
                    bat 'mvn clean install'
                }
            }
        }

        // Testing
        stage('Test') {
            steps {
                script {
                    bat 'mvn test'
                    junit 'target/surefire-reports/*.xml'
                    cucumber reportTitle: 'Cucumber Report', fileIncludePattern: 'target/example-report.json'
                }
            }
        }

//        stage('Deploy') {
//            steps {
//                script {
//                    bat 'mvn deploy'
//                }
//            }
//        }
    }


}