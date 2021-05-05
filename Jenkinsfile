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
				sh "docker-compose up -d --scale chrome=3"
				sh "docker-compose up -d --scale firefox=2"

			}
		}

		stage("Run Test"){
			steps{
				sh "docker-compose up SogetiTest SSLScan search-module"
			}
		}
	    stage("Archive results"){
		steps{
		    archiveArtifacts artifacts: 'output/**'
            step([$class: 'Publisher', reportFilenamePattern: '**/testng-results.xml'])

		}
		}
		stage('Allure report') {
            steps {
            sh 'sudo chmod -R o+xw output/allure-results'
            script {
                    allure([
                            includeProperties: false,
                            jdk: '',
                            properties: [],
                            reportBuildPolicy: 'ALWAYS',
                            results: [[path: 'output/allure-results']]
                    ])
            }
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

