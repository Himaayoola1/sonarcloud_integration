pipeline {
  agent any
  tools { 
        maven 'maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=haishat -Dsonar.organization=haishat -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=6ecc2d086da15d08fdc9f45078ebf3d5eebbcf49'
			}
    }

// 	stage('RunSCAAnalysisUsingSnyk') {
//             steps {		
// 				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
// 					sh 'mvn snyk:test -fn'
// 				}
// 			}
//     }	

// building docker image
stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("haishat_image")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
			
                    docker.withRegistry("https://543327957495.dkr.ecr.us-east-1.amazonaws.com", "ecr:us-east-1:aws-credentials") 
			{
                    app.push("latest")
                    }
                }
            }
    	}


  }
}
