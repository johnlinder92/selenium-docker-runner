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
				sh "docker-compose up search-module SogetiTest SSLScan"
			}
		}
	}
	post{
		always{
		    archiveArtifacts artifacts: 'output/**'
            step([$class: 'Publisher', reportFilenamePattern: '**/testng-results.xml'])
            steps {
                            script {
                                    allure([
                                            includeProperties: false,
                                            jdk: '',
                                            properties: [],
                                            reportBuildPolicy: 'ALWAYS',
                                            results: [[path: '/output/allure-results']]
                                    ])
                            }

                        }

			sh "docker-compose down"
			sh "sudo rm -rf output/"

		}
	}
}