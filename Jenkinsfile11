pipeline {
    agent {
        docker { image 'ubuntu' }
	}

     environment { 

   withCredentials([usernamePassword(credentialsId: 'jenkins_credentials', passwordVariable: 'password1', usernameVariable: 'username1')])
     }

    stages {
        stage('Run') {
            steps {
		
		 environment {

   withCredentials([usernamePassword(credentialsId: 'docker_login', usernameVariable: 'username1', passwordVariable: 'password1')])

    }
	
		sh 'set -x'
		sh 'date'

		sh "docker login -u ${username1} -p ${password1}"
	
		myImage.push("latest")		

		sh 'docker --version'

                sh "echo $username"
                sh "echo $password"
		sh 'docker images'
            }
        }
    }
}

