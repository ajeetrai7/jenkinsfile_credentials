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
	
               withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'github_credential', usernameVariable: 'username1',                   passwordVariable: 'password1']]) 				

			}
		steps{                    
                
                      sh '''
			set -x
			pwd
                        git clone  https://github.com/AJEETRAI707/122.git
              	        cd 122/
                        npm --no-git-tag-version version minor

                        git config --global user.name "ajeetrai707"
                        git config --global user.email ajeetrai707@gmail.com
                        git commit -am 'simple - commit '
                        git push origin master
                     '''
                  }



//			sh 'git clone https://github.com/AJEETRAI707/121.git'
//			sh 'cd 121/'
//			sh 'git init'
//			sh 'echo "Hello-World" > hello.txt'
//			sh 'ls |grep hello.txt'
//			sh 'git add hello.txt'
//		//	sh  'git commit -s -m '1st-commit''
			
//			}
	
	//	steps {
	//		script 'git commit -s -m "1st-commit" '
	//		}	
		}
		
	    }
     }


