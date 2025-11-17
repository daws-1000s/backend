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
        stage ('Deploy') {
            when {
                expression {env.GIT_BRANCH = "origin/main"}
            }
            steps {
                sh 'echo This is Deploy'
            }
        }
        stage ('print params') {
            steps {
                echo "Hello ${params.PERSON}"
                echo "Biography: ${params.BIOGRAPHY}"
                echo "Toggle: ${params.TOGGLE}"
                echo "Choice: ${params.CHOICE}"
                echo "Password: ${params.PASSWORD}"
            }
        }
        stage ('Approval') {
            input {
                message "shouls we continue?"
                ok "yes, we should."
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr jenkins', description: 'who should I say hello to?')
                }
            }
            steps {
                echo "Hello, ${PERSON}, nice to meet you."
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