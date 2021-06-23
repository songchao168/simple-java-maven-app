pipeline {
    agent any
	 environment{
        source_dir = "/var/jenkins_home/workspace/simple-java-maven-app/"
	   }
    stages {
	    stage ('Config'){
			steps {
			echo '配置环境变量'
			sh '''
				export JAVA_HOME="/usr/lib/java/jdk1.8.0_202/"
				export PATH="$JAVA_HOME/bin:$PATH"

				export MAVEN_HOME="/home/ubuntu/jenkins_home/install/apache-maven-3.8.1"
				export PATH="$MAVEN_HOME/bin:$PATH"
			   '''
			echo "JAVA_HOME: ${JAVA_HOME }"
			echo "MAVEN_HOME:' ${MAVEN_HOME}"
			sh '''
				java -version
				mvn -v
			   '''
			}
		}
        stage('Build') { 
            steps {
				sh 'cd ${source_dir}'
                sh 'mvn -B -DskipTests clean package' 	
            }
        }
	//stage('Test') {
    //        steps {
     //           sh 'mvn test'
     //       }
     //       post {
     //           always {
     //               junit 'target/surefire-reports/*.xml'
     //           }
     //       }
     //   }
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
