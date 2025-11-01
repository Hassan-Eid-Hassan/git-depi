pipeline {
    agent {
        label 'j-agent'
    }
    tools {
        maven 'maven-352'
    }
    stages{
        stage("Build"){
            steps{
                sh 'mvn package install'
            }
        }
        stage("Test"){
            steps{
                sh 'mvn test'
            }
        }
        stage("Archive"){
            steps{
                archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
            }
        }
    }
}