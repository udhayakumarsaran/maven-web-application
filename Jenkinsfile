node
{
    def mavenHome = tool name : "maven-3.8.1"
    stage('CHeckoutCOdefromSCM')
    {
      git branch: 'development', credentialsId: 'git-hub', url: 'https://github.com/udhayakumarsaran/maven-web-application.git'  
    }
    stage('buildpackageusingmaven')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('sonarreport')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('uploadartifactstonexus')
    {
        sh  "${mavenHome}/bin/mvn deploy"
    }
    stage('deploytoTOmcatserver')
    {
        sshagent(['ec2-user']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.126.178.77:/opt/apache-tomcat-9.0.46/webapps"
}
    }
    stage('sendingemailnotification')
    {
        emailext body: 'build completed - please check further', subject: 'Build status', to: 'devopsudhaya.kumar@gmail.com,splunkudhaya.kumar@gmail.com,udhayasaran@gmail.com'
    }
}
