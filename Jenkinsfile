pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    environment {
        SONAR_HOST_URL = 'http://sonarqube:9000'
        SONAR_TOKEN = 'sqp_d499aa95fc6b68d194a0f962b8d771bde497f245'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ahmedbenameur/cc.git'
                sh 'cp /var/jenkins_home/workspace/pom.xml .'
                sh 'pwd'  // Print workspace contents
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
                    // Download and install SonarQube Scanner
                    sh '''
                        if ! command -v sonar-scanner &> /dev/null; then
                            echo "SonarQube Scanner not found. Installing..."
                            wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
                            unzip -o -q sonar-scanner-cli-5.0.1.3006-linux.zip
                        fi
                    '''
                    // Verify SonarQube Scanner installation
                    sh '''
                        echo "PATH: $PATH"
                        ls -lR sonar-scanner-5.0.1.3006-linux
                        which sonar-scanner || echo "SonarQube Scanner not found in PATH"
                    '''
                }
                // Run SonarQube Scanner with updated PATH
                withEnv(["PATH+SCANNER=${WORKSPACE}/sonar-scanner-5.0.1.3006-linux/bin"]) {
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
