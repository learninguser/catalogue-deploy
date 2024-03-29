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
        string(name: 'version', defaultValue: '', description: "Artifact Version")
        string(name: 'environment', defaultValue: '', description: "Environment")
        choice(name: 'action', choices: ['apply', 'destroy'], description: "Create or destroy resource")
    }
    // Build stage
    stages {
        stage('Init'){
            steps {
                sh """
                    terraform init --backend-config=${params.environment}/backend.tf -reconfigure -upgrade
                """
            }
        }
        stage('Plan'){
            steps {
                sh """
                    terraform plan -var-file=${params.environment}/${params.environment}.tfvars -var="app_version=${params.version}"
                """
            }
        }
        stage('Apply changes'){
            when {
                expression { 
                    params.action == 'apply'
                }
            }
            input {
                message "Should we continue?"
                ok "Yes, we should."
            }
            steps {
                sh """
                    terraform apply -var-file=${params.environment}/${params.environment}.tfvars -var="app_version=${params.version}" -auto-approve
                """
            }
        }
    }
    // post build
    // post {
    //     always {
    //         deleteDir()
    //     }
    //     success {
    //         echo "Runs only if pipeline is succeded"
    //     }
    //     failure {
    //         echo "Runs only if pipeline is failed"
    //     }
    //     changed {
    //         echo "Runs only if there is change in state compared to previous"
    //     }
    // }
}