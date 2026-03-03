@Library('depi-r4')_

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
                script {
                    def mavenFuns = new io.depi.maven()
                    mavenFuns.JavaBuild("-DskipTests")
                }
            }
        }
        stage("Test Application") {
            steps {
                script {
                    def mavenFuns = new io.depi.maven()
                    mavenFuns.JavaTest("")
                }
            }
        }
        stage("Build Docker Image") {
            steps {
                script{
                    def dockerFuns = new io.depi.docker()
                    dockerFuns.build("hassaneid/depi-java", "v${BUILD_NUMBER}")
                }
            }
        }
        stage("Push Image") {
            steps {
                script{
                    def dockerFuns = new io.depi.docker()
                    dockerFuns.login("${DOCKER_USERNAME}", "${DOCKER_PASSWORD}")
                    dockerFuns.push("hassaneid/depi-java", "v${BUILD_NUMBER}")
                }
            }
        }
        stage("Deploy Docker Java App") {
            steps {
                sh """
                docker rm -f depi-java
                docker run -d -p 8090:8090 --name depi-java hassaneid/depi-java:v${BUILD_NUMBER}
                """
            }
        }
        stage("Deploy k8s Java App") {
            steps {
                sh """
                    sed -i "s#.*image:.*#    image: hassaneid/depi-java:v${BUILD_NUMBER}#g" deployment.yaml
                    cat deployment.yaml
                """
            }
        }
    }
}