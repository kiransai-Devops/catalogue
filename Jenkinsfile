pipeline {
    // this is pre-build section
    agent {
        node {
            label 'Agent-1'
        }
    }
    environment {
        COURSE = "jenkins"
        appVersion = ""
    }
    options {
        timeout(time :10, unit: 'SECONDS')
        disableConcurrentBuilds()
    }
    // this is build section
    stages {
        stage('read version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "app version : ${appVersion}"
                }  
            }
        }
        stage('dependies install') {
            steps {
                script {
                    sh """
                       npm install 

                    """
                }
                
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh """
                       echo "deploying"

                    """
                }
                
            }
        }
    }
    post {
        always {
            echo "i will run if success or not"
            cleanWs()
        }
        success {
            echo "i will run if success"
        }
        failure {
            echo "i will run if failure"
        }
        aborted {
            echo "pipeline is aborted"
        }
    }
}

