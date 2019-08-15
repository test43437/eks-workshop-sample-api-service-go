pipeline {
    agent any
    environment {
        
        DOCKER_IMAGE_NAME = "ashokshingade24/api-service-go"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                }
            }
        }
        stage('Push Docker Image') {
            
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('DeployToProduction') {
            
            steps {
                input 'Deploy to Production?'
                milestone(1)
                kubernetesDeploy(kubeconfigId: 'eks-kubeconfig',

                 configs: '**/hello-k8s.yml', '**/nginx.yml',              
                 enableConfigSubstitution: false,
        
                 secretNamespace: 'default',
                 secretName: 'hello-k8s',
                 dockerCredentials: [
                        [credentialsId: 'docker_hub_login'],
                 ]
)
            }
        }
    }
}
