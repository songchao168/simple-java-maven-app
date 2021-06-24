//任务前置准备操作（供以下情况参考：更换jenkins服务或jenkins容器重启时导致数据环境丢失时）
//1 进入jenkins容器安装OracleJDK8和Maven （apache-maven-3.8.1）
	1.1 docker exec -it root jenkins /bin/bash
	1.2 安装java 到/usr/lib/java/
	1.3 安装maven 到 /home/ubuntu/jenkins_home/install/
	1.4 修改maven  配置文件setting 中 localRepository 路径为 /home/ubuntu/jenkins_home/install/repository
//2 安装免密访问目标服务器的私钥到/var/jenkins_home/.ssh 目录， 此处的目标服务为部署应用服务的服务器，ip:47.106.186.226
//todo 后期计划支持部署服务到多节点，（可以在部署时由用户选择部署的节点？）

pipeline {
    agent any
	 environment{
        source_dir = "/var/jenkins_home/workspace/songchaoTestPipline"
	   }
    stages {
	    stage ('Build'){
			steps {
			echo '配置环境变量'
			echo "workspace: ${WORKSPACE}"
			sh '''
				export JAVA_HOME="/usr/lib/java/jdk1.8.0_202/"
				export PATH="$JAVA_HOME/bin:$PATH"
				export MAVEN_HOME="/home/ubuntu/jenkins_home/install/apache-maven-3.8.1"
				export PATH="$MAVEN_HOME/bin:$PATH"
				echo "JAVA_HOME: $JAVA_HOME"
				echo "MAVEN_HOME: $MAVEN_HOME"
				echo "login user is"
				whoami
				
				cd ${source_dir}
                mvn -B -DskipTests clean package
				
			   '''
			
			}
		}
    
        stage('Deliver') { 
            steps {
               
		  sh  'scp -vvv -i /var/jenkins_home/.ssh/wgs_rsa  ${source_dir}/target/my-app-1.0-SNAPSHOT.jar songchao@47.106.186.226:~/'
                  sh  '''
                      ssh -vvv -i /var/jenkins_home/.ssh/wgs_rsa  songchao@47.106.186.226 'cd  && touch t1234'
		           '''
            }
        }

    }
}
