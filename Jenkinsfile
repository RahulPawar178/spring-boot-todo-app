pipeline {
    agent any

    environment {
        // Define environment variables
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk'
        MAVEN_HOME = '/usr/share/maven'
        PATH = "${MAVEN_HOME}/bin:${JAVA_HOME}/bin:${PATH}"
        APP_NAME = 'spring-boot-todo-app'
        REGISTRY = 'docker.io'
        DOCKER_IMAGE = "${REGISTRY}/${env.BUILD_USER}/${APP_NAME}"
        BUILD_TIMESTAMP = sh(script: "date +'%Y%m%d_%H%M%S'", returnStdout: true).trim()
    }

    options {
        // Keep last 30 builds
        buildDiscarder(logRotator(numToKeepStr: '30'))
        // Add timestamps to console logs
        timestamps()
        // Timeout after 1 hour
        timeout(time: 1, unit: 'HOURS')
    }

    parameters {
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run unit tests')
        booleanParam(name: 'RUN_CODE_ANALYSIS', defaultValue: true, description: 'Run SonarQube analysis')
        booleanParam(name: 'BUILD_DOCKER_IMAGE', defaultValue: false, description: 'Build Docker image')
        booleanParam(name: 'DEPLOY_TO_DEV', defaultValue: false, description: 'Deploy to development environment')
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    echo "======== Checking out source code ========"
                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo "======== Building Spring Boot application ========"
                    sh '''
                        mvn clean compile \
                            -DskipTests \
                            -X
                    '''
                }
            }
        }

        stage('Unit Tests') {
            when {
                expression { params.RUN_TESTS == true }
            }
            steps {
                script {
                    echo "======== Running unit tests ========"
                    sh '''
                        mvn test \
                            -Dmaven.test.failure.ignore=true
                    '''
                }
            }
            post {
                always {
                    // Publish test results
                    junit 'target/surefire-reports/*.xml'
                    // Archive test results
                    archiveArtifacts artifacts: 'target/surefire-reports/**', allowEmptyArchive: true
                }
            }
        }

        stage('Code Quality Analysis') {
            when {
                expression { params.RUN_CODE_ANALYSIS == true }
            }
            steps {
                script {
                    echo "======== Running SonarQube analysis ========"
                    sh '''
                        mvn sonar:sonar \
                            -Dsonar.projectKey=${APP_NAME} \
                            -Dsonar.sources=src/main/java \
                            -Dsonar.tests=src/test/java \
                            -Dsonar.exclusions=**/config/**,**/entity/**
                    '''
                }
            }
        }

        stage('Package') {
            steps {
                script {
                    echo "======== Packaging application ========"
                    sh '''
                        mvn package \
                            -DskipTests \
                            -DfinalName=${APP_NAME}
                    '''
                }
            }
            post {
                success {
                    // Archive the built artifact
                    archiveArtifacts artifacts: 'target/*.jar', 
                                     fingerprint: true,
                                     onlyIfSuccessful: true
                }
            }
        }

        stage('Build Docker Image') {
            when {
                expression { params.BUILD_DOCKER_IMAGE == true }
            }
            steps {
                script {
                    echo "======== Building Docker image ========"
                    sh '''
                        docker build \
                            -t ${DOCKER_IMAGE}:${BUILD_NUMBER} \
                            -t ${DOCKER_IMAGE}:latest \
                            .
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            when {
                allOf {
                    expression { params.BUILD_DOCKER_IMAGE == true }
                    branch 'main'
                }
            }
            steps {
                script {
                    echo "======== Pushing Docker image to registry ========"
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', 
                                                      usernameVariable: 'DOCKER_USER', 
                                                      passwordVariable: 'DOCKER_PASS')]) {
                        sh '''
                            echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                            docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}
                            docker push ${DOCKER_IMAGE}:latest
                            docker logout
                        '''
                    }
                }
            }
        }

        stage('Deploy to Dev') {
            when {
                expression { params.DEPLOY_TO_DEV == true }
            }
            steps {
                script {
                    echo "======== Deploying to development environment ========"
                    sh '''
                        echo "Deployment script would go here"
                        echo "Example: kubectl set image deployment/todo-app todo-app=${DOCKER_IMAGE}:${BUILD_NUMBER}"
                    '''
                }
            }
        }
    }

    post {
        always {
            script {
                echo "======== Pipeline execution completed ========"
                // Clean up workspace if needed
                cleanWs()
            }
        }
        success {
            script {
                echo "======== Pipeline succeeded ========"
                // Send success notification
                // emailext(
                //     subject: "Build Success: ${APP_NAME} #${BUILD_NUMBER}",
                //     body: "The build completed successfully.",
                //     to: "team@example.com"
                // )
            }
        }
        failure {
            script {
                echo "======== Pipeline failed ========"
                // Send failure notification
                // emailext(
                //     subject: "Build Failed: ${APP_NAME} #${BUILD_NUMBER}",
                //     body: "The build failed. Please check the logs.",
                //     to: "team@example.com"
                // )
            }
        }
        unstable {
            script {
                echo "======== Pipeline unstable ========"
            }
        }
    }
}
