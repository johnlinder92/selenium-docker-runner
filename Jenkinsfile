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
				sh "docker-compose up search-module SSLScan SogetiTest"
			}
		}
	}
	post{
		always{
            step([$class: 'Publisher', reportFilenamePattern: 'test-output/testng-results.xml'])
			sh "docker-compose down"
			sh "sudo rm -rf output/"
		}
	}
}