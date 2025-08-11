pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('mock-aws-access-key')
        AWS_SECRET_ACCESS_KEY = credentials('mock-aws-secret-key')
        AWS_REGION = 'us-east-1' // Mocked AWS region
        DOCKER_IMAGE = "my-app:latest"
        K8S_DEPLOYMENT = "my-app-deployment"
        K8S_NAMESPACE = "default"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Registry') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag $DOCKER_IMAGE $DOCKER_USER/$DOCKER_IMAGE
                        docker push $DOCKER_USER/$DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl set image deployment/$K8S_DEPLOYMENT my-app=$DOCKER_USER/$DOCKER_IMAGE -n $K8S_NAMESPACE
                    kubectl rollout status deployment/$K8S_DEPLOYMENT -n $K8S_NAMESPACE
                '''
            }
        }

        stage('Mock AWS Deployment') {
            steps {
                echo "Skipping actual AWS deployment as AWS Free Tier limit exceeded."
                echo "Would normally run: aws s3 cp build/ s3://my-bucket-name --recursive --region $AWS_REGION"
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
