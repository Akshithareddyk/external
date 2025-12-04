pipeline {
    agent any
   
    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                bat 'python -m pip install -r requirements.txt'


                // Start Flask app
                bat 'start /B python app.py'

                // Wait for server
                bat 'timeout /t 5'

                // Run PyTest
                bat 'C:/Users/Admin/AppData/Local/Programs/Python/Python313/Scripts/pytest.exe -v'
            }
        }

        stage('Build Docker Image') {
            steps {
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



