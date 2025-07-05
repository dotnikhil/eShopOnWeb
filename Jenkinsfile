pipeline {
    agent any
    environment {
        FIMAGE = 'nikhildocker4424/eshop-frontend:1'
        BIMAGE = 'nikhildocker4424/eshop-backend:1'
        REPO = 'https://github.com/dotnikhil/eShopOnWeb.git'
    }
    stages {
        stage('git checkout') {
            steps {
                git branch: 'main', url: "${REPO}"
                echo 'Cloned the git repo'
            }
        }
        stage('docker login') {
            steps {
                withCredentials([string(credentialsId: 'dockerhubpass', variable: 'dockerhubpass')]) {
                    sh "docker login -u nikhildocker4424 -p $dockerhubpass"
                }
                echo 'Logged into DockerHub'
            }
        }
        stage('building image') {
            steps {
                echo 'Building image'
                sh "docker build -t $FIMAGE -f ./src/Web/Dockerfile ."
                sh "docker build -t $BIMAGE -f ./src/PublicApi/Dockerfile ."
            }
        }
        stage('pushing image') {
            steps {
                echo 'Pushing image to DockerHub'
                sh "docker push $FIMAGE"
                sh "docker push $BIMAGE"
            }
        }
        stage('k8s EKS Cluster') {
            environment {
                KUBECONFIG = '/home/config'
            }
            steps {
                echo 'Deploying pods to EKS'
                sh 'kubectl apply -f backend_deployment.yml'
                sh 'kubectl apply -f database_deployment.yml'
                sh 'kubectl apply -f frontend_deployment.yml'
                sh 'kubectl get pods -o wide'
            }
        }
    }
}
