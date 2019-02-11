pipeline{
	agent any
		stages{
			stage('Run'){
				sh 'pwd'
					}
			
			stage('Simple'){
				sh 'echo Hello-World'
				sh 'ls'
				}

			stage('Test'){
				environment{
					MY_NAME = 'AJEET'
					}
				sh 'printenv'
			
				}
			}
		}			
}
