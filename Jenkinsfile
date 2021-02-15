pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
  
   stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/amarpreets/webapp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    
    
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
    stage ('Deploy-to-tomcat'){
      steps {
        sshagent(['tomcat']){
          sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@13.127.195.81:/home/ubuntu/prod/apache-tomcat-8.5.63/webapps/webapp.war'
         
        }
      }
    }
    
 }
  
}
