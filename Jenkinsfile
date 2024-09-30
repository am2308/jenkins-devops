pipeline {
    // Define the agent where the pipeline will run (can be 'any' or specific nodes)
    agent any 

    // Environment variables that can be used throughout the pipeline
    environment {
        APP_NAME = 'my-application'
        DOCKER_IMAGE = 'my-app-docker-image'
        DEPLOY_ENV = 'staging'
    }

    stages {
        // Phase 1: Checkout - Get the latest code from the version control system (e.g., Git)
        stage('Checkout Code') {
            steps {
                echo 'Checking out code from Git repository...'
                git branch: 'main', url: 'https://github.com/am2308/jenkins-devops.git'
            }
        }

        // Phase 2: Build - Compile the code (e.g., with Maven, Gradle, npm)
        stage('Build') {
            steps {
                echo 'Building the application...'
                // sh './gradlew build' // For a Java application using Gradle
                // sh 'mvn clean install' // For a Maven build
                // sh 'npm install' // For a Node.js application
            }
        }

        // Phase 3: Test - Run unit tests or other test suites
        stage('Test') {
            steps {
                echo 'Running tests...'
                // sh './gradlew test' // Example: Running tests with Gradle
                // sh 'npm test' // For a Node.js project
            }
        }

        // Phase 4: Package - Package the application (e.g., JAR, WAR, Docker image)
        stage('Package') {
            steps {
                echo 'Packaging the application...'
                // sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_ID} ."
            }
        }

        // Phase 5: Security/Quality Scan (Optional) - Run security scans or code quality tools (e.g., Snyk, SonarQube)
        stage('Security Scan') {
            steps {
                echo 'Running security scans...'
                // Example: Using Snyk to check for vulnerabilities
                // sh 'snyk test'
            }
        }

        // Phase 6: Deploy to Staging - Deploy the application to the staging environment
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying the application to staging...'
                // sh "./deploy.sh ${DEPLOY_ENV} ${DOCKER_IMAGE}:${env.BUILD_ID}" // Custom deploy script
            }
        }

        // Phase 7: Automated Tests in Staging - Run smoke tests or end-to-end tests in the staging environment
        stage('Smoke Tests in Staging') {
            steps {
                echo 'Running smoke tests in staging...'
                // sh './smoke-test.sh' // Example script for automated tests
            }
        }

        // Phase 8: Deploy to Production - Deploy the application to the production environment
        stage('Deploy to Production') {
            // Add manual approval before deploying to production
            when {
                expression {
                    return input(message: 'Deploy to production?', ok: 'Deploy') // Requires manual approval
                }
            }
            steps {
                echo 'Deploying the application to production...'
                // sh "./deploy.sh production ${DOCKER_IMAGE}:${env.BUILD_ID}"
            }
        }
    }

    post {
        // Cleanup phase to remove temporary files, containers, or perform any other cleanup steps
        always {
            echo 'Performing cleanup...'
            // sh 'docker system prune -f' // Clean up Docker resources
        }

        // Send notifications or reports based on success or failure
        success {
            echo 'The pipeline executed successfully!'
            // Example: Notify team or send reports
            // emailext body: "Build Successful", to: "team@example.com"
        }

        failure {
            echo 'The pipeline failed!'
            // Example: Send failure notification
            // emailext body: "Build Failed", to: "team@example.com"
        }
    }
}
