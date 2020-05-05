pipeline
{
	agent any
	stages {
	
		stage('Build Application') {
			steps{
				script {
                        sh 'mvn -U clean install'
				}	
			}
		}
		
		stage('Deploy Application to CloudHub') {
			steps{
				script {
					sh 'mvn package deploy -DmuleDeploy'
				}
			}
		}	
		
	}
}