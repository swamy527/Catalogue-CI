pipeline {
    agent {
        node {
           label 'Agent-1'   
        }
    }
    environment {
        packageVersion = ''
        nexusurl = '52.23.187.24:8081'
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
        stage('publish-artifact') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusurl}",
                    groupId: 'com.roboshop',
                    version: "${packageVersion}",
                    repository: 'catalogue',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: catalogue,
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
            }
        }
    }
    post {
        always {
            deleteDir()
        } 
        success {
            echo 'I succeeded!'
        }
        failure {
            echo 'I failed :('
        }
    }
}