pipeline {
    agent {
        node {
           label 'Agent-1'   
        }
    }
    environment {
        packageVersion = ''
    }

    options {
        // Timeout counter starts AFTER agent is allocated
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
    }
    // parameters {
    //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
    // }
    stages {
        stage('version-scan') {
            steps {
                script {
                    def props = readJSON file: 'package.json'
                    packageVersion = props.version
                    echo "application version is ${packageVersion}"
                }
        }
        stage('Deploy') {
            steps {
                sh """
                     echo "Here are the environment variables"
                     sleep 10
                """
            }
        }
    }
    post {
        success {
            echo 'I succeeded!'
        }
        failure {
            echo 'I failed :('
        }
    }
}