pipeline {
    agent {
        label 'j-agent'
    }
    tools {
        jdk 'jdk-11'
        maven 'maven-352'
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
    }
}