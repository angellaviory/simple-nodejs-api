def branch = "master"
def repo = "git@github.com:angellaviory/simple-nodejs-api.git"
def cred = "ang"
def dir = "~/simple-nodejs-api"
def server = "ang@103.37.125.125"
def imagename = "simple"

pipeline {
	agent any
	stages {
		stage ('Pull From Git'){
		  steps{
		    sshagent ([cred]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
		    mkdir ${dir}
                    cd ${dir}
		    git init
                    git remote add origin ${repo}
		    git pull origin master
		    git branch ${branch}
		    git checkout ${branch}
		    git pull origin ${branch}
                    exit
                    EOF
                    """
	            }
               }
         }
	

         stage('Docker Run') {
            steps {
                sshagent([cred]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    mkdir ${dir}
                    cd ${dir}
                    docker run -d -p 3300:3000 ${imagename}:latest
                    exit
                    EOF
                    """
                    }
               }
         }

    }
}
