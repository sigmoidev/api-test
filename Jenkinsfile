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
                    recordCoverage(tools: [[parser: 'JACOCO']],
                        id: 'jacoco', name: 'JaCoCo Coverage',
                        sourceCodeRetention: 'EVERY_BUILD',
                        qualityGates: [
                                [threshold: 60.0, metric: 'LINE', baseline: 'PROJECT', unstable: true],
                                [threshold: 60.0, metric: 'BRANCH', baseline: 'PROJECT', unstable: true]])


                }
            }
        }

 stage('Documentation') {
            steps {
                script {
                    // Generate documentation (e.g., using Javadoc or a similar tool)
                    echo 'Generating documentation...'
                    // Replace with actual command to generate docs
                    sh 'mvn javadoc:javadoc'
                }
                // Archive the generated documentation
                archiveArtifacts artifacts: '**/target/site/apidocs/**', fingerprint: true
            }
        }
        stage('Archive') {
            steps {
                // Archive the artifacts from the build
                echo 'Archiving artifacts...'
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }






        stage('Deploy') {
            steps {
                script {
                    bat 'mvn deploy'
                }
            }
        }
    }


}