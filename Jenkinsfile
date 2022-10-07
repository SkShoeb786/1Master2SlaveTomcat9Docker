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
	}
}
