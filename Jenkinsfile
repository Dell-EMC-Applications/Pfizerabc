node
{
    def MavenHome = tool name: "maven3.6.3"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
    stage("Checkout code")
    {
        git branch: 'development', credentialsId: '3e8541bc-47af-4878-b4f7-900a5d99d1e6', url: 'https://github.com/Dell-EMC-Applications/Pfizerabc.git'
    }
    stage("Build")
    {
        sh "${MavenHome}/bin/mvn clean package"  
    }
    stage("ExecuteSonarqubeReport")
    {
        sh "${MavenHome}/bin/mvn sonar:sonar"
    }
    stage("UploadArtifact")
    {
        sh "${MavenHome}/bin/mvn deploy"
    }
    stage("DeployAppTomcat")
    {
        sshagent(['1421c726-3158-4e57-a9f0-10a378fca5a2']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@52.66.211.174://opt/apache-tomcat-9.0.34/webapps/"
}
    }
}
© 2020 GitHub, Inc.
Terms
Privacy
Security
Status
Help
Contact GitHub
Pricing
API
Training
Blog
About

