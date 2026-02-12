pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }



/* test*/ /****/
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


  stage('Build') {
            steps {
                script {
                    bat 'mvn clean install'
                }
            }
        }
        stage('Documentation') {
            steps {
                script {
                    bat 'mvn javadoc:javadoc'
                    // Clean up previous 'doc' folder if it exists
                    bat 'if exist doc rmdir /S /Q doc'
                    bat 'mkdir doc'
                    // Copy Javadoc content to the 'doc' folder
                    bat 'xcopy /E /I /Y target\\site doc'
                    // Delete existing doc.zip if it exists
                    bat 'if exist doc.zip del /Q doc.zip'
                    // Create the ZIP file with the new content
                    bat 'powershell -Command "Compress-Archive -Path doc\\* -DestinationPath doc.zip -Force"'
                    // Archive the doc.zip file for Jenkins artifacts
                    archiveArtifacts artifacts: 'doc.zip', fingerprint: true
                }
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




            stage("slack notification") {
                   steps {
                       bat '''
                           curl -X POST -H "Content-type: application/json" --data "{""text"":""Hello, World!""}" "%SLACK_WEBHOOK_URL%"
                       '''
                   }
               }


              stage('release') { steps {
              script {
/*test*/
              bat """
                 curl -X POST https://api.github.com/repos/sigmoidev/api-test/releases ^
                 -H "Authorization: Bearer ${TOKEN}" ^
                 -H "Accept: application/vnd.github+json" ^
                 -H "Content-Type: application/json" ^
                 -d "{\\"tag_name\\":\\"%VERSION%\\",\\"name\\":\\"Release %VERSION%\\",\\"body\\":\\"Production release\\",\\"draft\\":false,\\"prerelease\\":false}"
              """




              }
              } }
    }


}