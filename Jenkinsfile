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
        }
        stage('install-dependencies') {
            steps {
                sh """
                    npm install
                """
            }
        }
        stage('zip-folder') {
            steps {
                sh """
                    zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                    ls -l
                """
            }
        }
    }
    post {
        // always {
        //     deleteDir()
        // } 
        success {
            echo 'I succeeded!'
        }
        failure {
            echo 'I failed :('
        }
    }
}