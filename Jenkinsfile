pipeline{
    agent any
    environment{
		mavenHome = tool 'mymaven'
		dockerHome = tool 'mydocker'
		gitHome = tool 'myGit'
		PATH = "$dockerHome/bin:$mavenHome/bin:$gitHome/bin:$PATH"
	}
	stages{
		stage('Checkout'){
			steps{
				sh 'mvn --version'
				sh 'docker --version'
				echo "Build"
				echo "PATH - $PATH"
				echo "pwd - $PWD"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "JOB_NAME - $env.JOB_NAME"
			}
		}
        stage('Build '){
            steps{
                dir('./coit-frontend')
				echo "path- $PATH"
				script{
				checkout scm
				def dockerfile = 'Dockerfile-multistage'
				DockerFrontend = docker.build("kollidatta/frontend:${env.BUILD_TAG}","-f ${dockerfile}")
				//sh('docker build -t kollidatta/coitfrontend:v1 -f Dockerfile-multistage .')
				}
                
            }
        }
		stage('Push Docker Image'){
			steps{
				script{
					docker.withRegistry('','dockerhub'){
						DockerFrontend.push();
						DockerFrontend.push('latest');
					}
				}

			}
		}
		stage('Build backend2'){
            steps{
                dir('./coit-backend2'){
				echo "path- $PATH"
				script{
				def dockerfile = 'Dockerfile'
				DockerBackend2 = docker.build("kollidatta/backend2:${env.BUILD_TAG}")
				//sh('docker build -t kollidatta/coitbackend2:v1 -f ./coit-backend2/Dockerfile .')
				}
				}
                
            }
        }
		stage('Push Docker Image'){
			steps{
				script{
					docker.withRegistry('','dockerhub'){
						DockerBackend2.push();
						DockerBackend2.push('latest');
					}
				}

			}
		}
    }
	post {
			always{
				echo "Im awesome. I run always"
			}
			success{
				echo 'I un when you are successful'
			}
			failure{
				echo 'I run when you fail'
			}
		}
	

}