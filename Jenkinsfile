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
        ACC_ID = "765140436944"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"



    }
    options {
        // timeout(time :10, unit: 'SECONDS')
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
                       echo "npm installed"

                    """
                }
                
            }
        }
        stage('dependies install') {
            steps {
                script {
                    sh """
                       npm test
                    """
                }
                
            }
        }
        stage('docker image') {
            steps {
                script {
                    withAWS(region:'us-east-1',credentials:'AWS-cred') {
                        sh """
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                            docker images
                            docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}

                        """
                    }
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

