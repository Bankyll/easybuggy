pipeline{
    agent any 
    tools{
        maven "Maven_3_5_2"
    }
    stages{
        stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=techcorp -Dsonar.organization=techcorp -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=79ea801c1b86c73940e9ea9842a3db8e74b49857'
			}
        } 
	    stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }	
        stage('Build'){
            steps{
                withDockerRegistry(
                    [credentialsId:"dockerlogin", url: ""]
                )  {
                    script{
                    app = docker.build("tech365image")
                    }
                }
            }
        }

        stage('Push'){
            steps{
                script{
                    docker.withRegistry("https://251784464071.dkr.ecr.us-east-1.amazonaws.com", "ecr:us-east-1:aws-credentials"){
                        app.push("latest")
                    }
                    
                    
                }
            }
        }
    }

    
}
