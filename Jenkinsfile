// SCRIPTED
//DECLARATIVE
pipeline {
	 agent any
	// agent { docker { image 'maven: 3.6.3'} }
	// agent { docker { image 'maven:3.5.0-jdk-8'} }
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		// PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
		// je comprend pas 

	}

	stages {
		stage ('Checkout') {
			steps {
				sh 'mvn --version'
				sh 'docker version'
				echo "Build"
				echo "PATH - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "BUILD_URL - $env.BUILD_URL"

		}

		}
		stage('Compile') {
			steps {
				sh "mvn clean compile"

			}

		}
		
		stage('Test') {
			steps {
			sh " mvn test"
		}

		}
		stage('Integration Test') {
			steps {
				sh "mvn failsafe:integration-test  failsafe:verify"
		}

		}
		stage('Package') {
			steps {
				sh "mvnPackage -DskipTests"
		}

		}
		stage ('Buil Docker Image'){
			steps {
				// sh "docker buil -t bahalpha/ubuntu:$env.BUILD_TAG"
				script {
					// "docker.build -t bahalpha/ubuntu:$env.BUILD_TAG"
					script {
						dockerImage = docker.build("bahalpha/ubuntu:${env.BUILD_TAG}")
					}
				}
			}

		}
		stage ('Push Docker Image') {

			steps {
				script {
					docker.withRegistry('', 'dockerhub'){

						dockerImage.Push();
						dockerImage.Push('latest');

					}

				}

			}
		}
	} 
	
	post {

		always {
			echo 'Im awesome. I run always'
		}
		success {
			echo 'I run when you are successful'
		}
		failure {
			echo 'I run when you failure'
		}

	}

	
}
