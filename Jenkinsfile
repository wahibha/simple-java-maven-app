pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    
    environment {
       NEXUS_VERSION = "nexus3"
       NEXUS_PROTOCOL = "http"
       NEXUS_URL = "localhost:8081"
       NEXUS_CREDENTIAL_ID = "nexus_credentials"
    }

    options {
        skipStagesAfterUnstable()
    }

    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
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
         
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}
