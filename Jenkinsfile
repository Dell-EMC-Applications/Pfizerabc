node
{
    def MavenHome = tool name: "maven3.6.3"
    stage("checkout code")
    {
        git branch: 'development', credentialsId: '3e8541bc-47af-4878-b4f7-900a5d99d1e6', url: 'https://github.com/Dell-EMC-Applications/Pfizerabc.git'
    }
    stage("build")
    {
        sh "${MavenHome}/bin/mvn clean package"
    }
    stage("executesonarreport")
    {
        sh "${MavenHome}/bin/mvn sonar:sonar"
    }
    stage("uploadartifact")
    {
        sh "${MavenHome}/bin/mvn deploy"
    }
    /*stage("deployintomcat")
    {
        sshagent(['271a7c29-f2a9-4cff-8b38-64d56ddb0f87']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.206.100.134:/opt/apache-tomcat-9.0.34/webapps/"
    }
    }*/
    stage("creatingaimage")
    {
        sh "docker build -t vasanthsrinu/2ndimage ."
    }
    stage("Docker login and push")
    {
       withCredentials([string(credentialsId: 'Docker_Hub_password1', variable: 'DockerHubPassword1')]) {
       sh "docker login -u vasanthsrinu -p ${DockerHubPassword1}"
       }
       sh "docker push vasanthsrinu/2ndimage"
    }
    stage("Deploy application as container in docker deployement server")
    {
       sshagent(['1421c726-3158-4e57-a9f0-10a378fca5a2']) {
       sh "ssh -o StrictHostKeyChecking=no ec2-user@13.235.87.82 docker run -d -p 9090:8080 --name imagecontainer vasanthsrinu/2ndimage"
       } 
    }
}
