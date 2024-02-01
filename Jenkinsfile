
node{ 
    
    
echo "The Job name is: ${env.JOB_NAME}"
echo "The build number is: ${env.BUILD_NUMBER}"
echo "The Node name is: ${env.NODE_NAME}"
echo "The Home Dir is: ${env.JENKINS_HOME}"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM(' * * * * *')])])
def mavenHome = tool name: "maven3.9.5"
stage('CheckoutCode'){
git branch: 'development', credentialsId: '94c527e4-33de-4559-b46a-eb7d7ed3730c', url: 'https://github.com/mss-banking-projects-6am/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('UploadIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppIntoTomcat'){
sshagent(['a5bb5f02-3a0c-4fa6-8cba-d3b0382c42ed']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.28.108:/opt/apache-tomcat-9.0.85/webapps/"
}

}

}

