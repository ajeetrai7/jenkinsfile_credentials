pipeline{
	agent any
		stages{
		       	stage('Run'){
				steps{
	               			sh 'pwd'
						}
					}
	
			stage('Simple'){
				steps{
					sh 'echo Hello-World'
					sh 'ls'
					}
				}

			stage('Test'){
				environment{
					MY_NAME = 'AJEET'
					}
				steps{
				sh 'printenv'
			
				}
		}
	}
}			
