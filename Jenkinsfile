pipeline {
	agent  any 
    	   stages {
             stage('test') {
           	 steps {
	     		sh 'set -x'
                	echo "Hello-World"
			}
		}
		stage ('Building-credentials'){
		environment {
		withCredentials([usernamePassword(credentialsId: 'github_credential', usernameVariable: 'USERNAME1', passwordVariable: 'PASSWORD1')]) {
  		// available as an env variable, but will be masked if you try to print it out any which way
		  // note: single quotes prevent Groovy interpolation; expansion is by Bourne Shell, which is what you want
		  sh 'echo $PASSWORD1'
		  // also available as a Groovy variable
		  echo USERNAME
  		// or inside double quotes for string interpolation
  		echo "username is $USERNAME"
}
		}
		}
		stage('Simple-test'){
		steps{                    
                
			sh 'echo ${USERNAME1}'
			sh 'echo ${PASSWORD1}'
			sh 'git clone https://github.com/AJEETRAI707/121.git'
			sh 'cd 121/'
			sh 'git init'
			sh 'echo "Hello-World" > hello.txt'
			sh 'ls |grep hello.txt'
				 echo ${username1}
				 echo ${password1}
			}
		}
		stage ('Docker-credential'){
			steps {
				// This step should not normally be used in your script. Consult the inline help for details.
				withDockerRegistry(credentialsId: 'docker_login', url: 'docker push ajeetrai/ubuntu:tagname') {
    			sh "docker push ajeetrai/ubuntu:latest"

				}
			}
		}
		stage('Push to docker-hub'){
			steps {
        	docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
			}
		}
 
	}
}


