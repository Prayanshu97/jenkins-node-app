pipeline {
    agent any

    environment {
        IMAGE_NAME = "jenkins-node-app"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Prayanshu97/jenkins-node-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    # Remove old container if exists
                    docker rm -f node-container || true
                    # Run new container locally
                    docker run -d --name node-container -p 3000:3000 $IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }
    }

    post {
        always {
            echo "Build complete! Visit http://localhost:3000"
        }
    }
}
