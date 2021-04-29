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
        stage('Run tests in maven for report') {

         steps{
                        image 'maven:3-alpine'
                        sh "mvn clean test"
                   }

	}
	stage('reports') {
        steps {
        script {
                allure([
                        includeProperties: false,
                        jdk: '',
                        properties: [],
                        reportBuildPolicy: 'ALWAYS',
                        results: [[path: 'allure-results']]
                ])
        }
        }

	post{
		always{
            step([$class: 'Publisher', reportFilenamePattern: '**/testng-results.xml'])
			sh "docker-compose down"
			sh "sudo rm -rf output/"
		}
	}
}