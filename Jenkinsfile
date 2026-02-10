pipeline {
    agent any


    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/sigmoidev/deploy-api.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    bat 'mvn clean install -s settings.xml'
                }
            }

        }

        // testing
        stage('Test') {
            steps {
                script {
                    bat 'mvn test'
                    junit 'target/surfire-reports/*.xml'
                    cumcumber reportTitle: 'Cucumber Report',
                    fileIncludePattern: 'target/example-report.json',
                }
            }
        }





        stage('Deploy') {


         steps {
                        script {
                            bat 'mvn deploy'
                        }
                    }


}
}//            steps {
//                script {
//                    // Assuming your artifact is a jar file
//                    bat "mvn deploy:deploy-file -DgroupId=com.example -DartifactId=api -Dversion=1.0 -Dpackaging=jar -Dfile=target/api.jar -DrepositoryId=my-maven-repo -Durl=https://mymavenrepo.com/repo/eUWUNZTwakOS0mHhCZLq/ -Dusername=myMavenRepo -Dpassword=1234"
//                }
//            }
//        }
//    }
//
//    post {
//        success {
//            echo 'Deployment Successful!'
//        }
//        failure {
//            echo 'Deployment Failed!'
//        }
//    }
}