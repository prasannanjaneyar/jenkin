pipeline {
    agent any

    triggers {
        // Trigger on every git push
        pollSCM('* * * * *')   // every 1 min — required for GitHub PAT-based private repo
    }

    environment {
        ARM_CLIENT_ID        = credentials('AZURE_CLIENT_ID')
        ARM_CLIENT_SECRET    = credentials('AZURE_CLIENT_SECRET')
        ARM_TENANT_ID        = credentials('AZURE_TENANT_ID')
        ARM_SUBSCRIPTION_ID  = credentials('AZURE_SUBSCRIPTION_ID')
    }

    stages {
        stage('Checkout Repo') {
            steps {
                git branch: 'main',
                    credentialsId: 'git-pat',
                    url: 'https://github.com/your-org/your-repo.git'
            }
        }

        stage('Terraform Init') {
            steps {
                sh """
                    cd terraform
                    terraform init
                """
            }
        }

        stage('Terraform Validate') {
            steps {
                sh """
                    cd terraform
                    terraform validate
                """
            }
        }

        stage('Terraform Plan') {
            steps {
                sh """
                    cd terraform
                    terraform plan -out=tfplan
                """
            }
        }

        stage('Terraform Apply') {
            steps {
                sh """
                    cd terraform
                    terraform apply -auto-approve tfplan
                """
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
