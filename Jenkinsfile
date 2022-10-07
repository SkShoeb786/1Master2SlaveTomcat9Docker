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
		stage('slave1 permission'){
			agent{
				label{
					label'slave1'
				}
			}
			steps{
				sh 'sudo chmod -R 777 /home/ec2-user/docker-compose1.yaml'
			}
		}
		stage('slave2 permission'){
			agent{
				label{
					label'slave2'
				}
			}
			steps{
				sh 'sudo chmod -R 777 /home/ec2-user/docker-compose2.yaml'
			}
		}
		stage('deploy on slave1 and slave2 from master'){
			steps{
			sh 'scp -i /root/Common.pem /mnt/project/gameoflife-web/target/gameoflife.war ec2-user@13.232.95.184:/home/ec2-user/volume'
			sh 'scp -i /root/Common.pem /mnt/project/slave1/Dockerfile ec2-user@13.232.95.184:/home/ec2-user/slave1/'
			sh 'scp -i /root/Common.pem /mnt/project/docker-compose1.yaml ec2-user@13.232.95.184:/home/ec2-user/'
			sh 'scp -i /root/Common.pem /mnt/project/gameoflife-web/target/gameoflife.war ec2-user@35.154.148.252:/home/ec2-user/volume'
			sh 'scp -i /root/Common.pem /mnt/project/slave2/Dockerfile ec2-user@35.154.148.252:/home/ec2-user/slave2/'
			sh 'scp -i /root/Common.pem /mnt/project/docker-compose2.yaml ec2-user@35.154.148.252:/home/ec2-user/'
			}
		}
		stage('running docker on slave1'){
			agent{
				label{
					label'slave1'
				}
			}
			steps{
				sh 'sudo docker-compose -f docker-compose1.yaml down'
				sh 'sudo docker-compose -f docker-compose1.yaml up -d --scale service_1=2'
			}
		}
		stage('running docker on slave2'){
			agent{
				label{
					label'slave2'
				}
			}
			steps{
				sh 'sudo docker-compose -f docker-compose2.yaml down'
				sh 'sudo docker-compose -f docker-compose2.yaml up -d --scale service_2=2'
			}
		}
	}
}
