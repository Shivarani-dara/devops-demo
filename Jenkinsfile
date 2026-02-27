cat > ~/devops-demo/Jenkinsfile <<EOF
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'shivarani123/devops-demo'
    }

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Shivarani-dara/devops-demo'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t \${DOCKER_IMAGE} .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo \$PASSWORD | docker login -u \$USERNAME --password-stdin'
                    sh 'docker push \${DOCKER_IMAGE}'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}
EOF