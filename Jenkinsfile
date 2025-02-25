pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ahmedbenameur/cc.git'
               sh 'pwd && ls -lR'  // Print workspace contents
            }
        }

        stage('Check') {
            steps {
                sh 'python3 script.py'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                sh '''
                    mvn clean verify sonar:sonar \
                      -Dsonar.projectKey=sonar \
                      -Dsonar.projectName='sonar' \
                      -Dsonar.host.url=http://localhost:9000 \
                      -Dsonar.token=sqp_d499aa95fc6b68d194a0f962b8d771bde497f245
                '''
            }
        }
    }
}
