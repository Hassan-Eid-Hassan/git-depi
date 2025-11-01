pipeline {
    agent {
        label 'j-agent'
    }
    tools {
        jdk 'jdk-11'
        maven 'maven-352'
    }
    environment{
        dockerUsername = credentials("docker-user")
        dockerPassword = credentials("docker-pass")
    }
    stages{
        stage("Build"){
            steps{
                sh "java --version"
                sh 'mvn package install'
            }
        }
        stage("Archive"){
            steps{
                archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
            }
        }
        stage("Build Docker"){
            steps{
                sh "docker build -t hassaneid/java:v${BUILD_NUMBER} ."
            }
        }
        stage("Push Docker"){
            steps{
                sh "docker login -u ${dockerUsername} -p ${dockerPassword}"
                sh "docker push hassaneid/java:v${BUILD_NUMBER}"
            }
        }
        stage("Deploy"){
            steps{
                sh "docker run -d -p 8090:8090 --name java-v${BUILD_NUMBER} hassaneid/java:v${BUILD_NUMBER}"
            }
        }
    }
    post {
        always {
            cleanWs()
        }
        success {
            sh "echo "success""
        }
        failure {
            sh "echo "failure""
        }
    }
}