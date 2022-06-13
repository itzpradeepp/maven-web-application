node {
    def mavenHome=tool name:"Maven3.8.5"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    stage('checkoutCode')
    {
        git branch: 'development', credentialsId: 'b0bdc984-1cff-485a-a6dc-d3671636dc48', url: 'https://github.com/itzpradeepp/maven-web-application.git'
    }
    stage('BuildArtifacts')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('ExecuteSonarQubeReprot')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('UploadArtifactsToNexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('DeployToContainer')
    { sshagent(['f38464c1-10b8-45c2-8cd6-10c80d23b3a7']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.154.159.195:/opt/apache-tomcat-9.0.63/webapps"
}
        
}
}
