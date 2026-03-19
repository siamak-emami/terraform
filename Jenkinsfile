pipeline {
    agent any

    tools {
        terraform 'terraform-1.6'
    }

    environment {
        TF_IN_AUTOMATION = "true"
    }

    stages {


        stage('Terraform Init') {
            steps {
                sh '''
                    terraform version
                    terraform init
                '''
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out=tfplan'
            }
        }

        stage('Terraform Apply') {
            when {
                branch 'main'
            }
            steps {
                input message: 'Approve Terraform Apply?'
                sh 'terraform apply -auto-approve tfplan'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'tfplan', onlyIfSuccessful: true
        }
        failure {
            echo 'Terraform pipeline failed'
        }
    }
}
