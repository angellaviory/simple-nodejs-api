def branch = "main"
def repo = "git@github.com:angellaviory/simple-nodejs-api.git"
def cred = "ang"
def dir = "~/simple-nodejs-api"
def server = "103.37.125.125"
def imagename = "simple"


pipeline {
	agent any
	stages {
		stage ('Checkout Git'){
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
         stages {
                stage ('Build Docker Image'){
                  steps{
                    sshagent ([cred]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    mkdir ${dir}
                    cd ${dir}
                    docker build -t simple:latest .
                    exit
                    EOF
                    """
                    }
               }
         }
	 stages {
                stage ('Run Docker Image'){
                  steps{
                    sshagent ([cred]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    mkdir ${dir}
                    cd ${dir}
                    docker run -d -p 3000:3000 ${imagename}:latest 
                    exit
                    EOF
                    """
                    }
               }
         }
    
     }
}
