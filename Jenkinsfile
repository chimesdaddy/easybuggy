pipeline{
    agent any 
    tools{
        maven "Maven_3_5_2"
    }
    stages{
        stage('CompileandRunSonarAnalysis') {
            steps {	
		    withCredentials([string(credentialsId: 'SONAR_MEX', variable: 'SONAR_MEX')]) {
    sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=mexdevsec -Dsonar.organization=mexdevsec -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=$SONAR_MEX'
}

		
			}
        } 
	    stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'MEX_SNYK', variable: 'MEX_SNYK')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }	
        stage('Build'){
            steps{
                withDockerRegistry(
                    [credentialsId:"MEX_DOCKER", url: ""]
                )  {
                    script{
                    app = docker.build("mexnew")
                    }
                }
            }
        }

        stage('Push'){
            steps{
                script{
                    docker.withRegistry("https://211125785579.dkr.ecr.us-east-1.amazonaws.com", "ecr:us-east-1:MEX_AWS"){
                        app.push("latest")
                    }
                    
                    
                }
            }
        }
    }

    
}
