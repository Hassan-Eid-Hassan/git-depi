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
        stage('SonarQube Analysis') {
            steps{
                withSonarQubeEnv(credentialsId: 'sonar', installationName: 'SonarQube') {
                    sh "mvn clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=Depi -Dsonar.projectName='Depi'"
                }
            }
        }
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
        // stage("Push Image") {
        //     steps {
        //         script{
        //             def dockerFuns = new io.depi.docker()
        //             dockerFuns.login("${DOCKER_USERNAME}", "${DOCKER_PASSWORD}")
        //             dockerFuns.push("hassaneid/depi-java", "v${BUILD_NUMBER}")
        //         }
        //     }
        // }
        // stage("Deploy Docker Java App") {
        //     steps {
        //         sh """
        //         docker rm -f depi-java
        //         docker run -d -p 8090:8090 --name depi-java hassaneid/depi-java:v${BUILD_NUMBER}
        //         """
        //     }
        // }
        // stage("Deploy k8s Java App") {
        //     steps {
        //         sh """
        //             if [ -d "java-cd" ]; then cd java-cd && git pull; else git clone git@github.com:Hassan-Eid-Hassan/java-cd.git && cd java-cd; fi
        //             git config user.email "hassaneid339@gmail.com"
        //             git config user.name "Hassan-Eid-Hassan"
        //             sed -i "s#.*image:.*#        image: hassaneid/depi-java:v${BUILD_NUMBER}#g" java-app/deployment.yaml
        //             git add java-app/deployment.yaml
        //             git commit -m "change image version to v${BUILD_NUMBER} by jenkins"
        //             git push origin main
        //         """
        //     }
        // }
    }
}