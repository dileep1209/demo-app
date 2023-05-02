pipeline {
    agent any
    tools {
        maven 'maven'
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
                    
                    def release = 'simpleapp-release'
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'simple-app', 
                            classifier: '', 
                            file: "target/simple-app-${mavenPom.version}.war", 
                            type: 'war'
                        ]
                    ], 
                    credentialsId: 'Nexus_Cred', 
                    groupId: 'in.javahome', 
                    nexusUrl: '34.67.211.130:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: simpleapp-release, 
                    version: "${mavenPom.version}"
                    }
            }
        }
    }
}
