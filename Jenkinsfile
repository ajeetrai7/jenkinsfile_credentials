pipeline {
	agent  any 
    	   stages {
             stage('test') {
           	 steps {
	     		sh 'set -x'
                	echo "Hello-World"
			}
		}
		stage(build){
		    environment{
	
               withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'github_credential', usernameVariable: 'username', passwordVariable: 'password']]) 				
		
			echo ${username}
			echo ${password}

//			}
		stage('Simple-test'){
		steps{                    
                

			sh 'git clone https://github.com/AJEETRAI707/121.git'
			sh 'cd 121/'
			sh 'git init'
			sh 'echo "Hello-World" > hello.txt'
			sh 'ls |grep hello.txt'
			
			}
		}
		
	  }
   }
}
}


