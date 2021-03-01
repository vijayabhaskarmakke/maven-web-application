node
{

 def mavenHome = tool name: "maven3.6.3"
 
 properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '3')), pipelineTriggers([pollSCM('* * * * *')])])

stage ('CheckoutCode')
{
git branch: 'development', url: 'https://github.com/vijayabhaskarmakke/maven-web-application.git'
}

stage ('build')
{
    sh "${mavenHome}/bin/mvn clean package"
}

stage ('ExecuteSonarQubeReport')
{
    sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage ('UploadArtifactsIntoNexus')
{
    sh "${mavenHome}/bin/mvn deploy"
}

stage ('DeployAppIntoTomcatServer')
{
    sshagent(['0e8e633c-ae04-43a4-8bb0-95a4baf1d2eb']) 
    {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@54.167.192.207:/opt/apache-tomcat-8.5.63/webapps/"
    }
}

stage ('SendEmailNotification')
{
    emailext body: '''Build Over...

Regards,
Vijay,
9985623694''', subject: 'Build Over.', to: 'mvijayabhaskar383@gmail.com'
}
}
