```groovy
pipeline {
    agent any

    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                sh 'python3 -m pip install -r requirements.txt'

                sh 'nohup python3 app.py > app.log 2>&1 &'

                sh 'sleep 5'

                sh 'python3 -m pytest -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                sh "docker build -t seleniumdemoapp:v1 ."
            }
        }

        stage('Docker Login') {
            steps {
                echo "Docker Login"
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                }
            }
        }

        stage('push Docker Image to Docker Hub') {
            steps {
                echo "Push Docker Image to Docker Hub"

                sh "docker tag seleniumdemoapp:v1 varshitha256/mypythonflaskapp:seleniumtestimage"

                sh "docker push varshitha256/mypythonflaskapp:seleniumtestimage"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml --validate=false'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
```
