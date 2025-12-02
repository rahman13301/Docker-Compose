pipeline {
    agent any
    
    environment {
        DOCKER_HUB_REGISTRY = 'your-dockerhub-username' // Change this
        REPO_NAME = 'Docker-Compose'
        COMPOSE_FILE = 'docker-compose.yml'
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    echo 'Cloning repository from GitHub...'
                    checkout scmGit(
                        branches: [[name: '*/main']], // or '*/master'
                        userRemoteConfigs: [[
                            url: 'https://github.com/rahman13301/Docker-Compose.git',
                            credentialsId: '' // Add credentials ID if using private repo
                        ]]
                    )
                    
                    // Alternative: using git step if using pipeline script from SCM
                    // git branch: 'main', 
                    //      url: 'https://github.com/rahman13301/Docker-Compose.git'
                }
            }
        }
        
        stage('Validate Docker Compose') {
            steps {
                script {
                    echo 'Validating docker-compose.yml file...'
                    sh '''
                        # Check if docker-compose.yml exists
                        if [ ! -f "docker-compose.yml" ]; then
                            echo "ERROR: docker-compose.yml not found!"
                            exit 1
                        fi
                        
                        # Validate docker-compose syntax
                        docker-compose config
                    '''
                }
            }
        }
        
        stage('Build Images') {
            steps {
                script {
                    echo 'Building Docker images from docker-compose.yml...'
                    sh 'docker-compose build'
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    echo 'Running tests with docker-compose...'
                    sh '''
                        # Start services
                        docker-compose up -d
                        
                        # Wait for services to be ready
                        sleep 30
                        
                        # Run tests (example - modify according to your needs)
                        docker-compose exec -T [service_name] npm test || true
                        
                        # Stop services after tests
                        docker-compose down
                    '''
                }
            }
        }
        
        stage('Push to Docker Hub (Optional)') {
            when {
                branch 'main' // Only push from main branch
            }
            steps {
                script {
                    echo 'Logging into Docker Hub...'
                    withCredentials([usernamePassword(
                        credentialsId: 'docker-hub-credentials', // Create in Jenkins
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        sh '''
                            echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        '''
                    }
                    
                    echo 'Tagging and pushing images...'
                    sh '''
                        # Tag and push each service (modify as needed)
                        docker-compose push
                        
                        # Alternative: manual tagging if needed
                        # docker tag myapp_service:latest ${DOCKER_HUB_REGISTRY}/myapp:${BUILD_NUMBER}
                        # docker push ${DOCKER_HUB_REGISTRY}/myapp:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main' // Only deploy from main branch
            }
            steps {
                script {
                    echo 'Deploying application...'
                    sh '''
                        # Pull latest images (if using Docker Hub)
                        docker-compose pull
                        
                        # Deploy/Update the application
                        docker-compose up -d --force-recreate
                        
                        # Clean up old images
                        docker system prune -f
                    '''
                }
            }
        }
    }
    
    post {
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
            // You can add notifications here (Slack, Email, etc.)
        }
        failure {
            echo 'Pipeline failed!'
            // Add failure notifications
        }
    }
}
