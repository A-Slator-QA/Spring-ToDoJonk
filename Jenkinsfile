pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
               sh 'mvn clean test'
            }
        }
        stage('Build') {
            steps {
               sh '''
               mkdir -p /home/jenkins/appservice
echo '#!/bin/bash
sudo java -jar /home/jenkins/project-wars/project_build_${BUILD_NUMBER}.war' > /home/jenkins/appservice/start.sh
chmod +x /home/jenkins/appservice/start.sh
echo '[Unit]
Description=My SpringBoot App
[Service]
User=ubuntu
Type=simple
ExecStart=/home/jenkins/appservice/start.sh
[Install]
WantedBy=multi-user.target' > /home/jenkins/myApp.service
sudo mv /home/jenkins/myApp.service /etc/systemd/system/myApp.service

sudo systemctl daemon-reload    
sudo systemctl restart myApp
               mvn clean install
               mkdir -p /home/jenkins/project-wars
               mv ./target/*.war /home/jenkins/project-wars/project-${BUILD_NUMBER}.war
               '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                java -jar /home/jenkins/project-wars/project-${BUILD_NUMBER}.war
                '''
            }
        }
    }
}
