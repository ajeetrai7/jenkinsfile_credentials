// vi:set ts=2 shiftwidth=2 et
def ORG = "mayadataio"
def REPO = "cluster-register"
def DOCKER_HUB_REPO = "https://index.docker.io/v1/"
def BRANCH_NAME = BRANCH_NAME.toLowerCase()
env.user="atulabhi"
env.pass="ka879707"
  pipeline {
    agent {
      label {
        label ""
          customWorkspace "/var/lib/jenkins/workspace/${REPO}-${BRANCH_NAME}"
      }
    }
    stages {
      stage('Version') {
        steps {
          echo "updating the version"
           script {
            BN = sh(
               returnStdout: true,
               script: "./version ${REPO}/version.txt"
             ).trim()
          echo "${BN}"
           }
        }
      }
      stage('Build using dapper') {
        steps {
          echo "Workspace dir is ${pwd()}"
            script {
              GIT_SHA = sh(
                  returnStdout: true,
                  script: "git log -n 1 --pretty=format:'%h'"
                  ).trim()

                //echo "Checked out commit: ${env.GIT_COMMIT}"
                echo "Checked out branch: ${env.BRANCH_NAME}"
                sh(
                    script: "make ci",
                    returnStdout: true
                  )

            }
        }
      }

      stage('Retag the dapper image from HEAD-1234abc to the branch-1234abc') {
        steps {
          script {
            echo "Retag the image"
              TAG = sh (script: 'cat dist/images', returnStdout: true).trim()
              if (env.BRANCH_NAME == 'staging' || env.BRANCH_NAME.startsWith('alpha-r')) {
                sh(returnStdout: true, script: "docker tag ${TAG} ${ORG}/${REPO}:${BRANCH_NAME}-${GIT_SHA}" )
              } else if (env.BRANCH_NAME == 'master') {
                sh(returnStdout: true, script: "docker tag ${TAG} ${ORG}/${REPO}:${env.BN}")
              }
          }
        }
      }
      // TODO: (enhancement) Deprecate triggering when $BRANCH_NAME.mayaonline.io is up.
      stage('Publish image to DockerHub for builds which deploy') {
        steps {
          script {
            if (env.BRANCH_NAME == 'staging' || env.BRANCH_NAME.startsWith('alpha-r')) {
                sh "docker login --username=mayabot --password=M4Y4@openebs && docker push ${ORG}/${REPO}:${BRANCH_NAME}-${GIT_SHA}"
                echo "INFO: Copy the docker image sha to ${HOME}/${ORG}/${REPO}/${BRANCH_NAME}/"
                sh(returnStdout: true, script: "mkdir -p ${HOME}/${ORG}/${REPO}/${BRANCH_NAME}/")
                sh(returnStdout: true, script: "rm -f ${HOME}/${ORG}/${REPO}/${BRANCH_NAME}/*")
                sh(returnStdout: true, script: "touch ${HOME}/${ORG}/${REPO}/${BRANCH_NAME}/${BRANCH_NAME}-${GIT_SHA}")
            } else if (env.BRANCH_NAME == 'master') {
                sh(returnStdout: true, script: "docker login --username=mayabot --password=M4Y4@openebs && docker push ${ORG}/${REPO}:${env.BN}")

                sh(returnStdout: true, script: "mkdir -p ${HOME}/${ORG}/${REPO}/${BRANCH_NAME}/")
                sh(returnStdout: true, script: "rm -f ${HOME}/${ORG}/${REPO}/${BRANCH_NAME}/*")
                sh(returnStdout: true, script: "touch ${HOME}/${ORG}/${REPO}/${BRANCH_NAME}/${env.BN}")
            }
          }
        }
      }

      stage('Trigger maya-io-server for builds which deploy') {
        steps {
          script {
            if (env.BRANCH_NAME == 'staging' || env.BRANCH_NAME.startsWith('alpha-r')) {
              BUILD_JOB = sh(returnStdout: true, script: "echo ../maya-io-server/${BRANCH_NAME}").trim()
                build job: "${BUILD_JOB}", propagate: true, quietPeriod: 2, wait: true
            }
          }
        }
      }
      stage('Tag on sucessful build and push'){
        steps {
          script{
            if(env.BRANCH_NAME == 'master') {
              sh """
                       git tag -fa "${BN}" -m "Release of ${BN}"
                 """      
              sh "git tag -l"
              sh """
                      git push https://${env.user}:${env.pass}@github.com/mayadata-io/cluster-register.git --tag
                """
            }
          }
        }
      }
      stage('Push version tag to github'){
        steps {
          script{
            if(env.BRANCH_NAME == 'master') {
              sh(returnStdout: true, script: "./update.sh ${REPO} ${env.user} ${env.pass}")           
            }
          }
        }
      }
    }

    post {
      always {
        echo 'This will always run'
          deleteDir()
      }
      success {
        echo 'This will run only if successful'
          /*            slackSend channel: '#maya-io',
                        color: 'good',
                        message: "The pipeline ${currentBuild.fullDisplayName} completed successfully :dance: :thumbsup: "
           */
      }
      failure {
        echo 'This will run only if failed'
          slackSend channel: '#jenkins-builds',
                    color: 'RED',
                    message: "The pipeline ${currentBuild.fullDisplayName} failed. :scream_cat: :japanese_goblin: "
      }
      unstable {
        echo 'This will run only if the run was marked as unstable'
          /*            slackSend channel: '#maya-io',
                        color: 'good',
                        message: "The pipeline ${currentBuild.fullDisplayName} is unstable :scream_cat: :japanese_goblin: "
           */
      }
      changed {
        slackSend channel: '#jenkins-builds',
                  color: 'good',
                  message: "Build ${currentBuild.fullDisplayName} is now stable :dance: :thumbsup: "
                  echo 'This will run only if the state of the Pipeline has changed'
                  echo 'For example, if the Pipeline was previously failing but is now successful'
      }
    }
}
