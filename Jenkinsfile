pipeline {
    agent any
   
    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                    echo "Running Selenium Tests using pytest"
                    sh 'pip install -r requirements.txt'
                    sh 'start /B python app.py'
                    sh 'ping 127.0.0.1 -n 5 > nul'
                    sh 'python -m pytest -v'
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
                  sh 'docker login -u varshitha256 -p varshitha@L?1010'
                }
            }
        stage('push Docker Image to Docker Hub') {
            steps {
                echo "push Docker Image to Docker Hub"
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
