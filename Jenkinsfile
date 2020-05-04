pipeline
{
	agent any
	stages {
	
		stage('Build Application') {
			steps{
				script {
                    withMavenSettingsXml(mavenSettingsConfig: 'enterprise-settings',
                        image: config.mavenImage,
                        options: [artifactsPublisher(disabled: true)]) {
                        sh 'mvn -U clean install'
                    }
			}	
			
		}
		
		stage('Deploy Application to CloudHub') {
			steps{
				sh 'mvn package deploy -DmuleDeploy'
			}
		}	
	}
}