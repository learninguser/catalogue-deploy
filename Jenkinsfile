pipeline {
    agent {
        node {
            label 'agent-1'
        }
    }
    environment{
        projectName = "roboshop"
        component = "catalogue"
        nexusURL = "172.31.35.84:8081"
        packageVersion = ''
    }
    options {
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    parameters {
        string(name: 'version', defaultValue: '1.0.0', description: "Artifact Version")
        string(name: 'environment', defaultValue: 'dev', description: "Environment")
    }
    // Build stage
    stages {
        stage('Deploy'){
            steps {
                sh """
                    echo "version: ${params.version}"
                """
            }
        }
    }
    // post build
    post {
        always {
            deleteDir()
        }
        success {
            echo "Runs only if pipeline is succeded"
        }
        failure {
            echo "Runs only if pipeline is failed"
        }
        changed {
            echo "Runs only if there is change in state compared to previous"
        }
    }
}