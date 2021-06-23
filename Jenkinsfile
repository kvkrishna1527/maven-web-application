node('master')
or
node{
def mavenhome = tool name: "maven 3.6.3"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '5', numToKeepStr: '')), pipelineTriggers([pollSCM('* * * * *')])])
stage('check out code'){
git branch: 'development', credentialsId: '1252a808-d8a3-48f9-aac4-42abaf9dde34', url: 'https://github.com/kvkrishna1527/maven-web-application.git'
}
stage('build'){
sh "${mavenhome}/bin/mvn clean package"
}
stage('ExcuteSonarQubeReport'){
sh "${mavenhome}/bin/mvn sonar:sonar"
}
stage('UploadArctifactIntoNexus'){
sh "${mavenHome}/bin/mvn deploy"
}
stage('DeployAppIntoTomcatServer'){
sh "${mavenHome}/bin/mvn deploy"
sshagent(['4e12b156-11c7-4c12-915b-dd2d4ae145ab']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.207.117.252:/opt/apache-tomcat-9.0.46/webapps"
}
}
stage('SendEMailNotification'){
emailext body: '''build is over
regard
kvkrishna''', subject: 'build is over', to: 'kurra.vamsi@gmail.com'
}

}



