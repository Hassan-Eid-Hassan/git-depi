pipeline{
    agent {
        label 'j-agent'
    }
    stages{
        stage("Check SCM"){
            steps{
                git branch: 'main', url: 'https://github.com/Hassan-Eid-Hassan/git-depi.git'
            }
        }
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
    }
}