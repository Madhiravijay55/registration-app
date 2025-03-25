pipeline {
    agent any
    tools {
        jdk 'java17'
        maven 'Maven3'
    }
    #environment {
        APP_NAME    = "register-app-pipel"
        RELEASE     = "1.0.0"
        DOCKER_USER = "vijay065"          // Make sure this is a string
        DOCKER_PASS = 'dockerhub'         // This should match your Jenkins credential ID for Docker Hub
        IMAGE_NAME  = "${DOCKER_USER}/${APP_NAME}"
        IMAGE_TAG   = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Madhiravijay55/registration-app'
            }
        }
        stage("Build Application") {
            steps {
                sh "mvn clean package"
            }
        }
        stage("Test Application") {
            steps {
                sh "mvn test"
            }
        }
        stage("SonarQube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }
        stage("Quality Gate") {
            steps {
                script {
                    // Removed the unsupported credentials parameter
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        ##stage("Build & Push Docker Image") {
            steps {
                script {
                    def docker_image = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                    docker.withRegistry('', DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }
    }
}
