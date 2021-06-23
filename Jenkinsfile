pipeline {
    agent {
        docker {
            image 'maven:3.8.1-jdk-8' 
            args '-v /root/.m2:/root/.m2 -v /root/.ssh:/root/.ssh' 
           
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
		echo 'ls -a /home/1'
                sh  '''
		    cd /root/
                    pwd
                    ls -a	
			
		    '''
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
               // sh './jenkins/scripts/deliver.sh' 
		  sh  'scp -vvv  /var/jenkins_home/workspace/simple-java-maven-app/target/my-app-1.0-SNAPSHOT.jar songchao@47.106.186.226:~/'
                  sh  '''
                      ssh songchao@47.106.186.226 'cd  && touch t1234'
		      '''
            }
        }

    }
}
