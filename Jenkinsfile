pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time:30, unit: 'MINUTES')
        ansiColor('xterm')
    }
    environment {
        DEBUG = 'true'
        appVersion = ''
    }
    stages {
        stage ('Read the version') {
            steps {
                script {
                    def packageJSON = readJSON file: 'package.json'
                    appVersion = packageJSON.version
                    echo "APP version: ${appVersion}"
                }
            }
        }
        stage ('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage ('Dokcer build') {
            steps {
                sh """
                docker build -t mlndockerhub/backend:${appVersion} .
                docker images
            """    
            }
        }     
    }

    post {
        always {
            echo "This section runs always"
            deleteDir()
        }
        success {
            echo "This section runs when pipeline success"
        }
        failure {
            echo "This section runs when pipeline failed"
            echo "Thankyou for the opportunity."
        }
    }
}