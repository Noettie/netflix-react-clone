pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("732550944929.dkr.ecr.us-east-2.amazonaws.com/netflix_app:v1")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 732550944929.dkr.ecr.us-east-2.amazonaws.com/netflix_app:v1'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://732550944929.dkr.ecr.us-east-2.amazonaws.com/netflix_app', 'ecr:us-east-2:nottie-ecr') {
                    // build image
                    def myImage = docker.build("732550944929.dkr.ecr.us-east-2.amazonaws.com/netflix_app:v1")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
