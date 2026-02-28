pipeline{
    agent {
        label "agent-01"
    }
    tools {
        maven "maven-3-5-4"
        jdk "JDK-11"
    }
    environment {
        DOCKER_USERNAME = credentials("docker-username")
        DOCKER_PASSWORD = credentials("docker-password")
        DEPI_ROUND = "R4"
    }
    stages {
        stage("Build Application") {
            steps {
                sh "echo ${DEPI_ROUND}"
                sh "mvn package install -DskipTests"
            }
        }
        stage("Test Application") {
            steps {
                sh "mvn test"
            }
        }
        stage("Build Docker Image") {
            steps {
                sh "docker build -t hassaneid/depi-java:v${BUILD_NUMBER} ."
            }
        }
        stage("Push Image") {
                sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                sh "docker push hassaneid/depi-java:v${BUILD_NUMBER}"
            }
        }
    }
}