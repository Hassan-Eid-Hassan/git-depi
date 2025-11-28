pipeline{
    agent{
        label 'java-agent-01'
    }
    tools {
        maven 'maven-352'
        jdk 'java-11'
    }  
    stages{
        stage("Build App"){
            steps{
                sh 'java --version'
                sh 'mvn package install -DskipTests'
            }
        }
        stage("Archive App"){
            steps{
                archiveArtifacts artifacts: '**/*.jar'
            }
        }
        stage("Upload to nexus"){
            steps{
                nexusArtifactUploader(
                    artifacts: [[
                    artifactId: 'demo1',
                    classifier: '',
                    file: 'target/demo1-0.0.1-SNAPSHOT.jar',
                    type: 'jar']],
                    credentialsId: 'nexus-user-pass',
                    groupId: 'com.example',
                    nexusUrl: '192.168.61.142:8081',
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    repository: 'http://192.168.61.142:8081/repository/maven-snapshots/',
                    version: '0.0.1-SNAPSHOT'
                )
            }
        }
        // stage("Build Docker Image"){
        //     steps{
        //         sh "docker build -t hassaneid/sysadmin-java:v${BUILD_NUMBER} ."
        //     }
        // }
        // stage("Push Docker Image"){
        //     steps{
        //         sh "docker push hassaneid/sysadmin-java:v${BUILD_NUMBER}"
        //     }
        // }
    }
}