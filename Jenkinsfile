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
               mvn clean install
               mkdir -p /home/jenkins/project-wars
               mv ./target/*.war /home/jenkins/project-wars/project-${BUILD_NUMBER}.war
               '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                java -jar /jenkins/project-wars/project-${BUILD_NUMBER}.war
                '''
            }
        }
    }
}
