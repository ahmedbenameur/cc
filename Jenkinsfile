pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ahmedbenameur/my_application.git'
                sh 'ls -l'  // Print workspace contents
            }
        }

        stage('Check') {
            steps {
               sh 'python3 script.py'
            }
        }
