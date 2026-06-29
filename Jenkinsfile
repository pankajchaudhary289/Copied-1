pipeline
{
  agent any
  
  tools
  {
    maven 'Maven_3.8.2'
  }
  
  triggers
  {
    pollSCM('* * * * *')
  }
  
  options
  {
    timestamps()
    buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
  }
  
  stages
  {
    stage('Checkout Code from GitHub')
    {
      steps()
      {
        git branch: 'development', credentialsId: '957b543e-6f77-4cef-9aec-82e9b0230975', url: 'https://github.com/devopstrainingblr/maven-web-application-1.git'
      }
    }
    
    stage('Build Project')
    {
      steps()
      {
        sh "mvn clean package"
      }
    }
    
    stage('Execute SonarQube Report')
    {
      steps()
      {
        sh "mvn clean sonar:sonar"
      }
    }
    
    stage('Upload Artifacts to Sonatype Nexus')
    {
      steps()
      {
        sh "mvn clean deploy"
      }
    }
    
    stage('Deploy Application to Tomcat')
    {
      steps()
      {
        sshagent(credentials: ['90edf846-33cc-4ab4-a5f4-eca73823f716']) 
    {
      sh "scp -o StrictHostKeyChecking=no target/firstpc.war ec2-user@3.7.193.29:/opt/apache-tomcat-9.0.119/webapps"
        }
      }
    }
  }
