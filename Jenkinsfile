pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    environment {
        // Define SonarQube server URL and token
        SONAR_HOST_URL = 'http://sonarqube:9000'
        SONAR_TOKEN = 'sqp_d499aa95fc6b68d194a0f962b8d771bde497f245'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ahmedbenameur/cc.git'
                sh 'cp /var/jenkins_home/workspace/pom.xml .'
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
                script {
                    // Install SonarQube Scanner if not already installed
                    sh '''
                        if ! command -v sonar-scanner &> /dev/null; then
                            echo "SonarQube Scanner not found. Installing..."
                            wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
                            unzip sonar-scanner-cli-5.0.1.3006-linux.zip
                            export PATH=$PATH:$(pwd)/sonar-scanner-5.0.1.3006-linux/bin
                        fi
                    '''
                    // Run SonarQube Scanner
                    sh '''
                        sonar-scanner \
                          -Dsonar.projectKey=sonar \
                          -Dsonar.projectName=sonar \
                          -Dsonar.host.url=${SONAR_HOST_URL} \
                          -Dsonar.login=${SONAR_TOKEN} \
                          -Dsonar.sources=. \
                          -Dsonar.java.binaries=target/classes
                    '''
                }
            }
        }
    }
}
