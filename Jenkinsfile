pipeline{
	agent{
		label{
			label'built-in'
			customWorkspace'/mnt/project'
		}
	}
	stages{
		stage('build gameoflife on Master'){
			steps{
				sh 'mvn clean install -DskipTests'
			}
		}
		stage('deploy on slave1'){
			steps{
			sh 'scp -i /root/Common.pem /mnt/project/gameoflife-web/target/gameoflife.war ec2-user@13.232.95.184:/home/ec2-user/volume'
			sh 'scp -i /root/Common.pem /mnt/project/slave1/Dockerfile ec2-user@13.232.95.184:/home/ec2-user/slave1/'
			sh 'scp -i /root/Common.pem /mnt/project/docker-compose.yaml ec2-user@13.232.95.184:/home/ec2-user/'
			}
		}
		stage('running docker on slave1'){
			agent{
				label{
					label'slave1'
				}
			}
			steps{
				sh 'sudo docker-compose.yaml up -d --scale container_1=2'
			}
		}
	}
}
