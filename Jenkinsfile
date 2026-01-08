pipeline {
    agent any

    environment {
        DOCKER_CREDS = credentials('dockerhub-creds')
        SONAR_TOKEN  = credentials('sonar-creds')
        BACKEND_IMG  = "deena7/backend:latest"
        FRONTEND_IMG = "deena7/frontend:latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git credentialsId: 'github-creds',
                    url: 'https://github.com/Deena7/2tier-repo.git',
                    branch: 'main'
            }
        }

        stage('SonarQube Scan - Backend') {
    steps {
        dir('backend') {
            withCredentials([string(credentialsId: 'sonar-creds', variable: 'SONAR_TOKEN')]) {
                sh '''
                    export PATH=$PATH:/opt/sonar-scanner/bin
                    sonar-scanner \
                      -Dsonar.projectKey=backend \
                      -Dsonar.projectName=backend \
                      -Dsonar.sources=. \
                      -Dsonar.host.url=http://3.0.139.76:9000 \
                      -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }
    }
}

        stage('Build Backend Image') {
            steps {
                dir('backend') {
                    
                    sh "docker build -t ${BACKEND_IMG} ."
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                dir('frontend') {
                    sh '''
                       export DOCKER_BUILDKIT=0
                       docker build -t ${BACKEND_IMG} backend/
                       '''
                }
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh """
                    echo "$PASS" | docker login -u "$USER" --password-stdin
                    docker push ${BACKEND_IMG}
                    docker push ${FRONTEND_IMG}
                    """
                }
            }
        }

        stage('Deploy to k3s') {
            steps {
                sh "kubectl apply -f k3s/"
            }
        }
    }
}

