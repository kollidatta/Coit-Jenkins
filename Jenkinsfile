pipeline{
    agent any
    environment{
		mavenHome = tool 'mymaven'
		dockerHome = tool 'mydocker'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages{
		stage('Checkout'){
			steps{
				sh 'mvn --version'
				sh 'docker --version'
				echo "Build"
				echo "PATH - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "JOB_NAME - $env.JOB_NAME"
			}
		}
        stage('Build '){
            steps{
                dir('/coit-frontend')
                //DockerImage = docker.build("Dockerfile-multistage:${env.BUILD_TAG}")
            }
        }
		stage('Push Docker Image'){
			steps{
				script{
					docker.withRegistry('','dockerhub'){
						DockerImage.push();
						DockerImage.push('latest');
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