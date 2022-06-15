pipeline { 
  agent any
  tools {
    maven 'M2_HOME'
  }
  environment {
    registry = "okip/devops-ci"
    registryCredential = 'docker_user'
  }
  stages {
    stage('Build') {
      steps {
       sh 'mvn clean'
       sh 'mvn install'
       sh 'mvn package'
      }
    }
    stage('Test') {
      steps {
        echo "test step"
        sh 'mvn test'
      }
    }
    stage('Deploy') {
      steps {
        script {
          docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy on Tomcat') {
      steps {
        script {
          sshagent(['TomcatUserID']) {
            scp  -o StrictHostKeyChecking=no webapp/target/webapp.war ec2_user@18.212.169.223:/opt/tomcat/webapp
           }
         }
       }
     }
   }
}


