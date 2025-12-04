pipeline {
    agent any
   
    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                // Install Python dependencies
                bat 'pip3 install -r requirements.txt'

                // Start Flask app in background
                bat 'nohup python3 app.py &'

                // Wait a few seconds for the server to start
                bat 'sleep 5'

                // Run tests using pytest
                bat '/Users/akshithareddyk/Library/Python/3.9/bin/pytest -v'

            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                bat "docker build -t seleniumdemoapp:v1 ."
            }
        }

        stage('Docker Login') {
            steps {
                bat 'docker login -u akshithareddy27 -p docker123'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Push Docker Image to Docker Hub"
                bat "docker tag seleniumdemoapp:v1 akshithareddy27/sample:seleniumtestimage"
                bat "docker push akshithareddy27/sample:seleniumtestimage"
            }
        }

        stage('Deploy to Kubernetes') { 
            steps { 
                bat 'kubectl apply -f deployment.yaml --validate=false' 
                bat 'kubectl apply -f service.yaml' 
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


