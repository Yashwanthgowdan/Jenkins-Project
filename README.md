Jenkins CI/CD Pipeline for Flask App

This project demonstrates a complete CI/CD pipeline using Jenkins to build, test, and deploy a Flask application to an AWS EC2 instance.

Tech Stack:
Python 3.12
Flask
Pytest
Jenkins (Pipeline as Code)
AWS EC2 (Ubuntu)
GitHub
Email Notifications (via emailext)

Pipeline Stages
Checkout Code: Pulls the latest code from the main branch of GitHub.
Build: Creates a Python virtual environment.
       Installs dependencies from requirements.txt.
Test: Runs unit tests using pytest.
Deploy to EC2: Uses SSH to securely copy project files to a staging EC2 instance.

GitHub Webhook Setup (Auto Trigger)
To automatically trigger Jenkins builds when you push changes to GitHub:
Go to your GitHub repository → Settings → Webhooks.
Click "Add webhook".
In the Payload URL, enter your Jenkins GitHub webhook URL: <Add the necessary data>
Click Add webhook.

<img width="668" height="67" alt="image" src="https://github.com/user-attachments/assets/a8fe7924-460c-42cb-b275-d949c3272ab7" />

Notifications: Email notifications are sent on: Build start, Success, Failure

<img width="1318" height="233" alt="image" src="https://github.com/user-attachments/assets/5112eb4c-640f-4520-9136-ae26324db169" />
<img width="541" height="33" alt="image" src="https://github.com/user-attachments/assets/072e5518-b9c6-4fb6-bb4d-0daaf4cdf33b" />
