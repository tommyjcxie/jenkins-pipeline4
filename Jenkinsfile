pipeline {

    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        SSH_KEY               = credentials('ansible-key') // Use the ID of the SSH key credential
    }

   agent  any
    stages {
        stage('checkout') {
            steps {
                 script{
                
                       git branch: 'main', 
                         url: 'https://github.com/tommyjcxie/jenkins-pipeline4'
                       
                    }
                }
            }

        stage('Terraform Init and Plan') {
            steps {
                sh 'terraform init'
                // Generate the execution plan and save it to a file named tfplan
                sh 'terraform plan -out=tfplan'
            }
        }

        stage('Approval') {
            steps {
                script {
                    def plan = readFile 'tfplan'
                    input message: "Do you want to apply the Terraform plan?",
                          parameters: [text(name: 'Plan', description: 'Please review the plan', defaultValue: plan)]
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                // Apply the plan immediately
                sh 'terraform apply -input=false tfplan'
            }
        }
        
    }
}
