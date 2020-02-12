pipeline {
  agent any

environment {
     PATH = "/opt/apache-maven-3.6.3/bin:$PATH"
   }

  stages {
     stage("Git Checkout") {
	steps {
	   git credentialsId: 'github', url: 'https://github.com/naziralam/hello-world.git'
	}
      }

      stage("Build") {
	steps {
	   sh "mvn clean package"
	   sh "mv target/*.jar target/my-app.jar"
	}
      }

      stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
	stage("deploy") {
	  steps {
	   sshagent(['tomcat']) {

	   sh """
	       scp -o StrictHostKeyChecking=no target/my-app.jar ec2-user@172.31.43.87:/opt/tomcat8/webapps/
	       ssh ec2-user@172.31.43.87 /opt/tomcat8/bin/shutdown.sh
	       ssh ec2-user@172.31.43.87 /opt/tomcat8/bin/startup.sh
	     """
	   }
	  }
	}
      }
    }
