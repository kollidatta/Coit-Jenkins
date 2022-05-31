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
				//checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [],
				// gitTool: 'Default', userRemoteConfigs: [[url: 'https://github.com/kollidatta/Coit-Jenkins']]]) 
			}	
		}
        /*stage('Build'){
			steps{
				dir('./coit-frontend'){
                script{
                    def FRONTENDDOCKER = 'Dockerfile-multistage'
				    DockerFrontend = docker.build("kollidatta/frontend:${env.BUILD_TAG}","-f ${FRONTENDDOCKER} .")
			}
            
        	 }
            }
        }*/
		stage('K8s Deployment'){
			steps{
			dir('./resource-manifests'){
					script{
						kubernetesDeploy(
							configs:"coit-frontend-deployment.yaml",
							kubeconfigId:"k8s",
							enableConfigSubstitution:true
						)
						}
					}
    			}
		}
		stage('K8s Deployment for lb'){
			steps{
			dir('./resource-manifests'){
					script{
						kubernetesDeploy(
							configs:"service-coit-frontend-lb.yaml",
							kubeconfigId:"k8s",
							enableConfigSubstitution:true
						)
						}
					}
    			}
		}
	}
}
