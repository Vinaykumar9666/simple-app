pipeline {
    agent any
    tools {
        maven 'maven3.6.3'
    }
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
    }
    stages{
        stage('Build'){
            steps{
                 sh script: 'mvn clean package'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                    def mavenPom = readMavenPom file: 'pom.xml'
                    def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "simpleapp-snapshot" : "simple-app-release"
                    nexusArtifactUploader artifacts: [[artifactId: 'simple-app', classifier: '', file: 'target/simple-app-3.0.0', type: 'war']], credentialsId: 'nexus', groupId: 'in.javahome', nexusUrl: '192.168.134.210', nexusVersion: 'nexus3', protocol: 'http', repository: 'simple-app-release', version: '3.0.0'
                }
            }
        }
    }
}
