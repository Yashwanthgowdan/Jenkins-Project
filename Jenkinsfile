pipeline {
    agent any

    environment {
        REPO = "https://github.com/Yashwanthgowdan/Jenkins-Project.git"
        SSH_cred_ID = "Yashwanth"
        EC2_USERNAME = "ubuntu"
        EC2_IP = "34.220.39.203"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: "${env.REPO}"
            }
        }
        
        stage('Build') {
            steps {
            sh '''
            echo "Creating virtual environment..."
            python3 -m venv venv
            echo "Activating virtual environment and installing dependencies..."
            . venv/bin/activate
            pip install --upgrade pip
            pip install -r requirements.txt
            '''
                
            }
            
        }

        stage('Test') {
            steps {
                sh '''
                echo "Running unit tests in virtual environment..."
                . venv/bin/activate
                pytest
                '''
            }
        }

        stage('Deploy to Staging') {
            when {
                expression {
                    return currentBuild.currentResult == 'SUCCESS'
                }
            }
            steps {
                sshagent(credentials: [env.SSH_cred_ID]) {
                    sh """
                        echo "Deploying to staging EC2..."
                        scp -o StrictHostKeyChecking=no -r ${env.WORKSPACE}/* ${env.EC2_USERNAME}@${env.EC2_IP}:/home/ubuntu/
                    """
                }
            }
        }
    }

    post {
        failure {
            echo "❌ Build failed. Check console output for errors."
        }
        success {
            echo "✅ Pipeline completed successfully!"
        }
    }
}
