pipeline{
	agent any
	stages{
		stage("Pull Latest Image"){
			steps{
				sh "docker pull johnarturlinder/selenium-docker"
			}
		}
		stage("Start Grid"){
			steps{
				sh "docker-compose up -d hub chrome firefox"
			}
		}
		stage("Run Test"){
			steps{
				sh "docker-compose up search-module"
				step([$class: 'Publisher', reportFilenamePattern: '/var/jenkins_home/workspace/testng-results.xml'])
			}
		}
	}
	post{
		always{

			sh "docker-compose down"
			sh "sudo rm -rf output/"
		}
	}
}