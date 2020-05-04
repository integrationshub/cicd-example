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
				withMavenSettingsXml(mavenSettingsConfig: 'enterprise-settings',
                        image: config.mavenImage,
                        options: [artifactsPublisher(disabled: true)]) {
                        def PROJECT_VERSION=sh(returnStdout: true, script: "mvn help:evaluate -Dexpression=project.version").trim()
                        echo "publishing ${PROJECT_VERSION} to artifactory"

                        sh 'mvn package deploy -DmuleDeploy'
                    }
			}
		}	
	}
}