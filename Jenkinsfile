@Library('depi')_
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
                script{
                    def depiMaven = new edu.iti.maven()
                    depiMaven.maven("package install")
                }
            }
        }
        stage("Archive"){
            steps{
                archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
            }
        }
        stage("Build Docker"){
            steps{
                script{
                    def depiDocker = new edu.iti.docker()
                    depiDocker.build("hassaneid/java", "v${BUILD_NUMBER}")
                }
            }
        }
        stage("Push Docker"){
            steps{
                script{
                    def depiDocker = new edu.iti.docker()
                    depiDocker.login("${dockerUsername}", "${dockerPassword}")
                    depiDocker.push("hassaneid/java", "v${BUILD_NUMBER}")
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
        success {
            echo "success"
        }
        failure {
           echo "failure"
        }
    }
}