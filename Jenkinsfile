pipeline {
    agent any // Uses any available Jenkins node
    
    environment {
        SONAR_URL = "http://localhost:9000" // SonarQube server URL (update if needed)
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository
                git branch: 'main', url: 'https://github.com/NithishCodes-2411/React-Jenkins-K8s-CICD.git'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install' // Install dependencies
                sh 'npm run build' // Build the project
                sh 'ls' // List the files in the root directory to confirm everything is there
            }
        }

        stage('Static Code Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonarkubeAuthToken', variable: 'SONAR_AUTH_TOKEN')]) {
                    script {
                        // Install dependencies and perform static code analysis
                        sh '''
                            # Install dependencies
                            npm install

                            # Run SonarQube analysis with sonar-scanner
                            sonar-scanner \
                                -Dsonar.projectKey=my-react-app \
                                -Dsonar.projectName="My React App" \
                                -Dsonar.projectVersion=1.0 \
                                -Dsonar.sources=src \
                                -Dsonar.host.url=$SONAR_URL \
                                -Dsonar.login=$SONAR_AUTH_TOKEN
                        '''
                    }
                }
            }
        }

        stage('Upload to Docker Hub') {
            environment {
                DOCKER_IMAGE = "nithishdocker2411/reactapp:${BUILD_NUMBER}"
                REGISTRY_CREDENTIALS = credentials('docker-cred')
            }
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t ${DOCKER_IMAGE} .'

                    // Push Docker image to Docker Hub
                    def dockerImage = docker.image("${DOCKER_IMAGE}")
                    docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes deployment and service configurations
                    sh 'kubectl apply -f Kubernetes/deployment.yaml'
                    sh 'kubectl apply -f Kubernetes/service.yaml'
                }
            }
        }
    }
}
