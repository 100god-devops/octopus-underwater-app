pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
         stage('Clone repository') {
            steps {
                script{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'f804c6ec-98c3-4029-8dd9-66b4434f33e5', url: 'https://github.com/saurabhchauhan177/octopus-underwater-app.git']]])
                }
            }
        }

        stage('Build') {
            steps {
                script{
                 app = docker.build("870165402940.dkr.ecr.us-east-1.amazonaws.com/octopus-underwater-app")
                }
            }
        }
        stage('Push') {
            steps {
                script{
                        docker.withRegistry(
                            'https://870165402940.dkr.ecr.us-east-1.amazonaws.com', 
                            'ecr:us-east-1:aws-credentials') 
                            {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
                stage('Deploy'){
            steps {
                 sh 'kubectl apply -f deployment.yml'
            }
        }

    }
}
