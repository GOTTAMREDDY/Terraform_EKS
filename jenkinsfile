pipeline {
    agent any
    parameters {
        choice(name: 'action', choices: ['apply', 'destroy'], description: 'Terraform action')
    }
    stages {
        stage('Clone the Code') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/GOTTAMREDDY/Terraform_EKS.git']]
                )
            }
        }
        stage('Terraform Initialization') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Terraform Validation') {
            steps {
                sh 'terraform validate'
            }
        }
        stage('Infrastructure Checks') {
            steps {
                sh 'terraform plan'
                input(message: "Approve?", ok: "proceed")
            }
        }
        stage('Create/Destroy EKS cluster') {
            steps {
                sh "terraform ${params.action} --auto-approve"
            }
        }
    }
}
