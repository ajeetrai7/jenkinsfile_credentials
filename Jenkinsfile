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
	
               withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'github_credential', usernameVariable: 'username1', passwordVariable: 'password1']]) 				
		
			echo ${username1}
			echo ${password1}

//			}
		stage('Build'){
		steps{                    
                

			sh 'git clone https://github.com/AJEETRAI707/121.git'
			sh 'cd 121/'
			sh 'git init'
			sh 'echo "Hello-World" > hello.txt'
			sh 'ls |grep hello.txt'
		//	sh  'git commit -s -m '1st-commit''
			
			}
		}
		
	  }
   }
}


