node
{
def mavenHome = tool name: "maven3.6.3"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '3', numToKeepStr: '')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckoutCode')
{
git branch: 'development', credentialsId: 'eb3e5c76-4d54-4713-8552-7972641c1f41', url: 'https://github.com/vijayabhaskarmakke/maven-web-application.git'
}
 
stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactIntoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppIntoTomcatServer')
{
sshagent(['a7624b3c-4a67-43d3-b689-94c5064990a1']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@100.25.103.144:/apache-tomcat-8.5.64/webapps"
}
}

}
