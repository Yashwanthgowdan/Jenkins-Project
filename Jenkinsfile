pipeline {
    agent any

    environment {
        REPO = "https://github.com/Yashwanthgowdan/Jenkins-Project.git"
        SSH_cred_ID = "Yashwanth"
        EC2_USERNAME = "ubuntu"
        EC2_IP = "34.220.39.203"
    }

    stages {

        stage('Notify Start') {
            steps {
                emailext (
                    subject: "Build Started: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    body: "The build has started.\nCheck it here: ${env.BUILD_URL}",
                    to: 'yashyashwanthgowdan@gmail.com'
                )
            }
        }

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
        success {
            emailext (
                subject: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "✅ Build succeeded.\nCheck details: ${env.BUILD_URL}",
                to: 'yashyashwanthgowdan@gmail.com'
            )
        }
        failure {
            emailext (
                subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "❌ Build failed.\nCheck logs: ${env.BUILD_URL}",
                to: 'yashyashwanthgowdan@gmail.com'
            )
        }
        always {
            echo "Build completed with result: ${currentBuild.currentResult}"
        }
    }
}
